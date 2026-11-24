# inutri — data model (first version)

The single source for tables, fields and access rules. Every task that touches data builds on this file; a change to it goes in its own pull request first. Database: PostgreSQL on Supabase (EU). Migrations live in `supabase/migrations/`.

Conventions: `id uuid` primary keys, `created_at timestamptz default now()`, dates as `date`, times in UTC (shown in Europe/Lisbon). Health data never leaves these tables except to the professional's website and the patient's own app.

## People and access

| Table | Fields | Notes |
|---|---|---|
| `professionals` | `id` (= `auth.users.id`), `name`, `email` | One row per nutritionist. Today only one; built for several (team management later). |
| `patients` | `id`, `professional_id` → professionals, `auth_user_id` (nullable until the invite is accepted), `name`, `email`, `on_glp1 bool`, `status` (`invited` · `active` · `deactivated`), `invited_at`, `activated_at`, `deactivated_at`, `feedback_weekday` (0–6), `fat_mass_display` (`kg` · `percent`), `muscle_mass_display` (`kg` · `percent`), `next_consult_date` (nullable) | Each patient has exactly one professional. Deactivated: login blocked, all data kept, can be reactivated. |

## Private clinical record (professional only, never readable by the app)

| Table | Fields |
|---|---|
| `clinical_records` | `patient_id` (PK) → patients, `birth_date`, `profession`, `phone`, `height_cm`, `doctor`, `history`, `usual_medication`, `allergies`, `lab_results`, `habits`, `weight_history`, `goals` |
| `clinical_notes` | `id`, `patient_id`, `consult_date`, `text` |
| `clinical_attachments` | `id`, `patient_id`, `name`, `storage_path` (bucket `clinical`) |

## Foods and products

| Table | Fields | Notes |
|---|---|---|
| `foods` | `id`, `name`, `section` (`fruta` · `legumes` · `cereais` · `laticinios` · `carne_peixe_ovos` · `leguminosas` · `gorduras` · `bebidas` · `outros`), `source` (`insa` · `edited` · `manual`), `insa_code` (nullable), and per 100 g: `kcal`, `protein_g`, `carbs_g`, `fat_g`, `fibre_g` | Editing an INSA food sets `source = edited`. No AI suggestions in the first version. |
| `food_supermarkets` | `food_id`, `supermarket` (`continente` · `pingo_doce` · `lidl` · `aldi` · `mercadona` · `intermarche`) | Marked by hand by the professional. |
| `products` | `barcode` (PK), `name`, `food_id` (nullable) → foods, `status` (`linked` · `pending`), `linked_at` | A barcode is a specific product linked to a generic food. `pending` = scanned by a patient, waiting for the professional to link it. Linked once, it works for every patient. |
| `product_scans` | `id`, `patient_id`, `barcode`, `scanned_at` | Unknown scans show up in reports. |

## Plans

| Table | Fields | Notes |
|---|---|---|
| `plans` | `id`, `patient_id`, `start_date`, `end_date`, `medication_weekday` (nullable, GLP-1 only) | One plan has many versions. Plans of one patient never overlap: a new plan starts after the current one ends, or ends it. |
| `plan_versions` | `id`, `plan_id`, `version int`, `status` (`draft` · `published`), `note_to_patient`, `published_at` | Editing creates a draft; publishing replaces what the app shows. Every published version stays as history. |
| `plan_meals` | `id`, `plan_version_id`, `name`, `position` | |
| `plan_items` | `id`, `plan_meal_id`, `food_id`, `grams` | |

**The app shows** the latest published version of the plan whose dates include today. After `end_date` it shows "À espera do teu novo plano".

**Totals** come from the database view `plan_version_totals` (per meal and per day: kcal, protein, carbs, fat, fibre, and % of energy from protein, carbs and fat with 4/4/9 kcal per gram), so the website and the app show the same numbers.

## Measurements and doses

| Table | Fields | Notes |
|---|---|---|
| `measurements` | `id`, `patient_id`, `measured_on`, `kind` (`weight` · `waist` · `hip` · `fat_mass` · `muscle_mass`), `value numeric(6,1)`, `unit` (`kg` · `cm` · `percent`), `recorded_by` (`professional` · `patient`), `status` (`confirmed` · `pending`), `confirmed_at` | Weight in kg; waist and hip in cm; fat and muscle mass in kg **or** %, saved with the unit the person used. Charts show the patient's display unit and convert with the weight from the same day; with no same-day weight, the value is shown in its recorded unit. A patient's entry is `pending` until the professional confirms or corrects it. |
| `doses` | `id`, `patient_id`, `taken_on`, `medication` (e.g. semaglutido, tirzepatido), `dose_mg numeric(5,2)` | Recorded only by the professional. The app shows them; it never advises. |

## Weekly feedback

| Table | Fields | Notes |
|---|---|---|
| `feedback` | `id`, `patient_id`, `week_start`, `week_end`, `on_glp1` (copied from the patient when sent), `answers jsonb`, `submitted_at`, `read_at`, `reply` (nullable), `replied_at` | One per patient per week. The week closes on the patient's `feedback_weekday`. The professional can write **one short reply** per feedback; the patient sees it in the app. Not a chat. |

The questions are in [decisoes.md](decisoes.md) ("Perguntas do feedback semanal"). **States** come from the database function `feedback_state(question, value)`:
- Days out of 7: **Correu bem** 5–7, **A construir** 3–4, **Ficou para a próxima** 0–2. Reversed for "comer entre refeições, fora do plano" (0–2 is *Correu bem*).
- Three-option answers: best option → *Correu bem*, middle → *A construir*, worst → *Ficou para a próxima*.
- Free text, weight and perimeter have no state.

## Documents, consent and requests

| Table | Fields | Notes |
|---|---|---|
| `documents` | `id`, `patient_id`, `name`, `storage_path` (bucket `documents`), `uploaded_at`, `seen_at` | `seen_at` null = shown as "Novo". |
| `consents` | `id`, `patient_id`, `text_version`, `accepted_at` | No app use without a current consent. |
| `data_requests` | `id`, `patient_id`, `kind` (`export` · `delete`), `created_at`, `handled_at` | Arrive on the website; handled by the professional. |

## Access rules (row-level security)

- **Professional:** reads and writes every row of her own patients (`patients.professional_id = auth.uid()`), including the clinical record.
- **Patient:** reads only her own rows, and **never** `clinical_records`, `clinical_notes` or `clinical_attachments`. She reads plans (published versions only), documents, measurements, doses, her feedback and replies.
- **Patient can write only:** `measurements` (as `pending`, `recorded_by = patient`), `feedback` (her own, before the week closes), `product_scans`, `consents`, `data_requests`, `documents.seen_at`.
- **Foods and products:** readable by every signed-in user; written only by professionals (patients create `pending` products only through `product_scans`).
- **Storage:** bucket `documents` readable by the owning patient and her professional; bucket `clinical` by the professional only.
- Tests in `supabase/tests/` prove each rule, above all: a patient cannot read another patient's data, and the app can never read the clinical record.
