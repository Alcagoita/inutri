# inutri — rules for the AI

Read by every AI session in this project. Built on Brush Frame: https://github.com/Alcagoita/brush-frame

## Owners and decisions
The project has **two owners with equal say on the product**: Olegário Nascimento (GitHub `Alcagoita`) and Inês Nascimento (GitHub `inesmcn27-bit`). Either owner is a **decider**:
- **Decisions:** decisions are made in a brainstorming session with whichever owner is in it. **That owner's decision is final**: it needs no input, review or confirmation from the other owner. Write what was decided, who decided, the date and why in [docs/decisoes.md](docs/decisoes.md) (and in this file when it changes a rule, a promise or a model fact), then turn it into execution tasks. Never create a decision ticket, never write "to be decided by the project owner" and never leave a decision waiting for the other owner.
- **Ticket assignee:** every ticket is assigned to the person who created it (the owner in the session). When you create a ticket, set the assignee to the current Jira user; when you find one without an assignee, assign it to its reporter.
- **Pull requests:** every pull request needs one approval from the owner who did not open it. After that approval, either owner can say merge.
- **GitHub:** the repository belongs to Olegário's account, so only he is a GitHub admin (repository settings, merging without an approval). Inês has write access, which is enough to approve and to merge after approval.

Both owners speak Portuguese (pt-PT): answer in the language they write in. Code, commits and pull requests are in English. The full product picture for the team is in [docs/visao-do-produto.md](docs/visao-do-produto.md).

## What this product is
A nutrition follow-up tool for weight-loss patients, with or without GLP-1 agonist medication. It has two sides: the **professional website**, where the nutrition professional builds each patient's plan, adds support documents and tools, and follows evolution charts (weight and body measurements); and the **patient mobile app**, where the patient reads their plan and their own data, fills a weekly feedback and plan reports, and gets a reminder on medication day.

## Product promises
A change that breaks one needs an owner's decision first, recorded in [docs/decisoes.md](docs/decisoes.md).
- **Only the professional creates or changes a plan.** The patient can read it, never edit it.
- **Every plan is built by hand for one patient.** Never generate, copy or suggest a plan automatically.
- **Only the professional adds support documents and tools.** The patient can read them, never add or change them.
- **No medical advice.** The app never recommends doses, medication or treatment. For GLP-1 it only records the doses the professional enters and reminds the patient on the day of the dose.

## Model facts
- Each patient has exactly one professional.
- A plan has a start and an end. A patient follows one plan at a time.
- A plan is **one typical day** (meals with foods and quantities in grams), built by the professional in a spreadsheet that adds up energy and macronutrients from the food database. The food database starts from the INSA food composition table (licence to confirm), can be edited, takes foods added by hand, and records in which supermarkets the professional finds each food.
- Feedback is weekly, with a fixed set of questions per group: one set for patients on GLP-1, another for patients without. The questions are listed in [docs/decisoes.md](docs/decisoes.md) ("Perguntas do feedback semanal"). Behaviours come first; weight and abdominal perimeter come last and are optional.
- Whether a patient is on GLP-1 is recorded by the professional, and it changes the plan as well as the feedback questions.
- For GLP-1 patients, the professional records each dose with its date, so doses can be compared over time with weight and body-measurement results. The patient sees the recorded doses in the app; the app never advises on doses or symptoms.
- Measurements are weight, waist (abdominal perimeter), hip, fat mass and muscle mass. The professional records them on the website and the patient can record them in the app; a patient's entry reaches the professional's reports "to confirm".
- Each patient has a **private clinical record** (personal and clinical data from the first visit, plus notes from each visit). Only the professional sees it; it is never sent to the app.
- Patient accounts are created only by the professional (email invite). Notification and email texts never carry health data.
- Weight, body measurements and medication are health data: the most sensitive kind of personal data (GDPR special category).
- Built with **Kotlin Multiplatform** (Android and iOS). The professional side is a **website** (decided in INU-2). The website and the app share one server, which stores health data in Europe.

## Architecture
Decided by Olegário Nascimento on 24 November 2026 (see [docs/decisoes.md](docs/decisoes.md), "arquitetura técnica").
- **Backend:** Supabase in the EU region (Frankfurt): PostgreSQL, Auth, Storage and row-level security. Two projects: **test** (made-up data only) and **production**. Every patient row belongs to one professional; row-level security enforces it, and the patient API can never read the private clinical record.
- **Shared rules live in the database** (plan totals, feedback questions), so the app and the website compute the same results.
- **Website:** React + TypeScript with the official Supabase client. Where it is hosted is still open (prefer Europe); it stores no health data itself.
- **App:** Kotlin Multiplatform (Android and iOS) with the Supabase Kotlin client.
- **Professional login (website):** email and password, then a passkey (fingerprint, Touch ID, Windows Hello) as the second step, with a code by email as fallback. Session lasts 7 days on the same computer; "Sair" ends it at once. Confirm Supabase Auth supports these factors before building; if not, bring it back to an owner.
- **Patient login (app):** email and password, no sign-up. The professional creates the account; the patient gets an email and sets the password on a small web page that only sets and resets passwords. The app keeps the session and asks for Face ID, fingerprint or the phone code when opened.
- **Email:** Brevo (EU) as Supabase's email sender, for invites, password recovery and codes.
- **Errors:** Sentry EU region in the app and the website; never send health or personal data.
- **Offline:** the app reads the plan, shopping guide and documents offline, stored encrypted on the phone and wiped on logout; feedback and measurements queue until online.
- **Reminders:** set on the phone (local notifications), not pushed by a server: the medication-day reminder, and a weekly feedback reminder on the day the professional sets per patient (that day closes the feedback week).
- **Plan end:** after its end date a plan disappears from the app ("À espera do teu novo plano"); past plans stay on the website as history.
- **Barcodes:** a barcode is a product linked to a generic food. The professional links common products on the website; an unknown product scanned by a patient arrives in reports and, once linked, works for every patient.

## First version
Decided by Inês Nascimento (INU-2 and INU-4 on 23 November 2026; scope closed on 24 November 2026, see [docs/decisoes.md](docs/decisoes.md)). Execution tasks: INU-5 to INU-22. Anything outside this list needs an owner's decision first.
- **Professional website:** login with two-step verification; patient list, create patients and invite them to the app; one page per patient (summary and evolution, plan, documents, private clinical record); food database (INSA table, edits, foods added by hand, supermarkets per food); plan as a typical day in a spreadsheet with automatic totals, published to the app; record measurements and GLP-1 doses with dates and see evolution charts; upload support documents; reports page with weekly feedback, patient measurements (confirm or correct) and patient questions.
- **Patient app (Android and iOS):** login and password recovery (account created by the professional); home; read the plan and support documents; weekly feedback (GLP-1 or non-GLP-1 questions) and the behaviours being kept; evolution with recorded doses, and recording weight and measurements; medication-day reminder; shopping guide by supermarket; barcode reader that says whether a product is in the guide and in which meal of the plan.
- **Both:** consent and privacy screens before any real patient data.
- **Later, not in the first version:** food diary with meal photos and AI calorie estimates; website team management (inviting other professionals or administrators); messages between professional and patient (the open question at the end of the weekly feedback covers this for now).

## Brand and tone
Decided in INU-3 (Inês Nascimento, 23 November 2026). The app's name is still open.
- **Colours (watermelon palette):** rind green `#1F5135` (main: headings, buttons); lime `#A8D13C` (accents only: icons, achievements); light green `#DDEFC8` (backgrounds, "went well" state); salmon `#F28C7C` (human moments: messages from the professional, celebrations). Support colours: cream `#FBF6EE` (page background), seed `#2E2A26` (body text), honey `#F2C26B` ("building" state), light salmon `#FCE3DD` (professional's message cards).
- **Contrast:** lime and salmon are never text colours on a light background; text is rind green or seed.
- **Weekly feedback tone: curiosity, not judgement.** Three states, never red: *Correu bem*, *A construir*, *Ficou para a próxima*. Behaviours kept come before the weight chart. Compare a patient only with themselves, never with others. Show trends, not scores ("Mantiveste 4 dias, mais 1 do que na semana passada", never "Falhaste 3 dias"). End with an open question to the professional.
- **Two voices:** sober and clear on the professional side; warm and encouraging in the patient app.
- **Words never used:** "milagre", "rápido", "dose recomendada", or anything else that promises results or gives medical advice.

## Workflow
- **The board holds execution tasks only:** work with a visible result in the app or the website. Decisions come out of brainstorming (see Owners and decisions) and become execution tasks; they are never tickets themselves.
- Tasks live in Jira, project **INU**: https://olegarioncnascimento.atlassian.net/jira/software/projects/INU. No work without a task.
- The board has four columns: **To do, In development, Testing, Done**. Move the task yourself: **In development** when you start, **Testing** when the pull request opens, **Done** when it merges. If Jira can't be reached from here, tell the person which column to move it to.
- Every commit and pull request title starts with the task's reference: `INU-12: …`. A hook and a CI check enforce it. Setup work uses `INU-0`.
- Checks `test` and `ticket-key` must pass. AI review (CodeRabbit) is installed; on this repository it reviews only when asked (tick **Trigger review** on the PR, or comment `@coderabbitai review`). Commit and push its fixes straight away.

## Gitflow: you run it, the person never has to
The project has two protected lanes: `develop`, where finished work comes together, and `main`, what is published. Nobody pushes to either directly; everything arrives through a pull request, and only a decider (either owner) says merge. You handle every branch below without being asked. Don't ask the person to choose or name a branch, and don't explain Gitflow unless they ask: say what happened in one plain line ("A tua mudança está à espera do teu OK aqui: <link>").

**A task (feature or fix):**
1. Update `develop` (`git switch develop && git pull`).
2. Branch from it: `INU-12-short-description`.
3. Commit with the reference, push, open the pull request into `develop`.
4. Every pull request needs one approval from the other owner. Merge only after that approval and on a decider's word (`gh pr merge <n> --merge`); the branch is deleted after the merge. Use `--admin` only when a decider explicitly says to merge without an approval.

**A release** (when the decider says to publish):
1. From an up-to-date `develop`, branch `release/X.Y.Z`. Pick the version yourself (patch for fixes, minor for features, major only on the decider's word) and say which.
2. On it: bump the version where the project keeps it, and write the release notes from the tasks merged since the last tag, grouped by area.
3. Pull request `release/X.Y.Z` into `main`. On the decider's word, merge.
4. Tag `main`: `git tag -a vX.Y.Z -m "Release vX.Y.Z"` and push the tag; create the release with its notes (`gh release create vX.Y.Z --notes-file …`).
5. Pull request `release/X.Y.Z` back into `develop`, so the version bump isn't lost; merge it, then delete the release branch.

**An urgent fix to what's published** (hotfix):
1. Only when something published is broken and can't wait for the next release, and the decider agrees.
2. Branch `hotfix/short-description` from `main`, with a task for it.
3. Pull request into `main`; merge on the decider's word, tag the next patch version (`v1.4.2` → `v1.4.3`) and release it.
4. Pull request the same branch into `develop` too, so the fix isn't lost; then delete it.

**Never:** push to `main` or `develop`, force-push a shared branch, rewrite published history, delete `main` or `develop`, or merge without the decider's word.

## Rules
1. One ticket per branch and per pull request. Several tickets and pull requests can be open at the same time across the team: start the next ticket without waiting for open pull requests to merge. Wait only when the new ticket needs work that is still in an unmerged pull request; then say which one it waits for.
2. The ticket is the spec. Nothing outside it. If something is unclear, propose a change to the ticket instead of guessing.
3. Check the ticket's real status in Jira before starting. Never trust a copy.
4. Never merge on your own. Merge only when a decider (either owner) says so, after one approval from the other owner.
5. Unit tests for any logic, before the PR. Not during quick visual tweaks.
6. Small slice first: run any job over a lot of data on a sample, check, then widen.
7. When something breaks, ask what the person already knows before hunting. Root cause first, then the fix, then the tests.
8. Never put keys, tokens or passwords in code, prompts, chat or this file. They live in local files that git ignores.
9. Decisions go in [docs/decisoes.md](docs/decisoes.md) and in this file, never only in chat: the next session cannot see the chat.
10. Never use real patient data in tests, examples, fixtures, screenshots or prompts. Use made-up data only.
11. Never add medical, medication or dose advice to the app, in any text, notification or feature.
12. Never send health data to a service hosted outside Europe. Check where a service stores data before adding it.

## Memory
Corrections that should hold for everyone become rules here, under Corrections, with why, the ticket and a review date. Decisions go in [docs/decisoes.md](docs/decisoes.md). Personal preferences stay in each person's own tool memory. Never store ticket status, keys or tokens in any of them.

## Corrections
<!-- Each as: the rule — Why: … — Source: ticket, date — Review by: date. -->
- **No decision tickets on the board.** Decisions are made in brainstorming and recorded in docs/decisoes.md; the Jira board holds only execution tasks with a visible result in the app or the website. — Why: the board should show the work being built, not discussions. — Source: Inês Nascimento, INU-0, 23 November 2026 — Review by: 23 February 2027.
