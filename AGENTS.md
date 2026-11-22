# inutri — rules for the AI

Read by every AI tool in this project. `CLAUDE.md` imports this file, so Claude Code and Codex follow the same rules. Built on Brush Frame: https://github.com/Alcagoita/brush-frame

The owner speaks Portuguese (pt-PT): talk to them in Portuguese. Code, commits and pull requests are in English. The full product picture for the team is in [docs/visao-do-produto.md](docs/visao-do-produto.md).

## What this product is
A nutrition follow-up tool for weight-loss patients, with or without GLP-1 agonist medication. It has two sides: the **professional side**, where the nutrition professional builds each patient's plan, adds support documents and tools, and follows evolution charts (weight and body measurements); and the **patient mobile app**, where the patient reads their plan and their own data, fills a weekly feedback and plan reports, and gets a reminder on medication day.

## Product promises
A change that breaks one needs a decision ticket first.
- **Only the professional creates or changes a plan.** The patient can read it, never edit it.
- **Every plan is built by hand for one patient.** Never generate, copy or suggest a plan automatically.
- **Only the professional adds support documents and tools.** The patient can read them, never add or change them.
- **No medical advice.** The app never recommends doses, medication or treatment. For GLP-1 it only records the doses the professional enters and reminds the patient on the day of the dose.

## Model facts
- Each patient has exactly one professional (today: the owner).
- A plan has a start and an end. A patient follows one plan at a time.
- Feedback is weekly, with a fixed set of questions per group: one set for patients on GLP-1, another for patients without.
- Whether a patient is on GLP-1 is recorded by the professional, and it changes the plan as well as the feedback questions.
- For GLP-1 patients, the professional records each dose with its date, so doses can be compared over time with weight and body-measurement results.
- Weight, body measurements and medication are health data: the most sensitive kind of personal data (GDPR special category).
- Built with **Kotlin Multiplatform** (Android and iOS). Where the professional side runs (desktop, web or the same app) is still open: decision ticket INU-2.

## Workflow
- Tasks live in Jira, project **INU**: https://olegarioncnascimento.atlassian.net/jira/software/projects/INU. No work without a task.
- The board has four columns: **To do, In development, Testing, Done**. Move the task yourself: **In development** when you start, **Testing** when the pull request opens, **Done** when it merges. If Jira can't be reached from here, tell the person which column to move it to.
- Every commit and pull request title starts with the task's reference: `INU-12: …`. A hook and a CI check enforce it. Setup work uses `INU-0`.
- Checks `test` and `ticket-key` must pass. AI review (CodeRabbit) is installed; on this repository it reviews only when asked (tick **Trigger review** on the PR, or comment `@coderabbitai review`). Commit and push its fixes straight away.

## Gitflow: you run it, the person never has to
The project has two protected lanes: `develop`, where finished work comes together, and `main`, what is published. Nobody pushes to either directly; everything arrives through a pull request, and only the decider says merge. You handle every branch below without being asked. Don't ask the person to choose or name a branch, and don't explain Gitflow unless they ask: say what happened in one plain line ("A tua mudança está à espera do teu OK aqui: <link>").

**A task (feature or fix):**
1. Update `develop` (`git switch develop && git pull`).
2. Branch from it: `INU-12-short-description`.
3. Commit with the reference, push, open the pull request into `develop`.
4. Merge only on the decider's word (`gh pr merge <n> --merge --admin`); the branch is deleted after the merge.

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
1. One ticket at a time. Do not start the next until the current PR is merged.
2. The ticket is the spec. Nothing outside it. If something is unclear, propose a change to the ticket instead of guessing.
3. Check the ticket's real status in Jira before starting. Never trust a copy.
4. Never merge on your own. Merge only when the decider says so: the owner.
5. Unit tests for any logic, before the PR. Not during quick visual tweaks.
6. Small slice first: run any job over a lot of data on a sample, check, then widen.
7. When something breaks, ask what the person already knows before hunting. Root cause first, then the fix, then the tests.
8. Never put keys, tokens or passwords in code, prompts, chat or this file. They live in local files that git ignores.
9. Decisions go in tickets and in this file, never only in chat: the next session cannot see the chat.
10. Never use real patient data in tests, examples, fixtures, screenshots or prompts. Use made-up data only.
11. Never add medical, medication or dose advice to the app, in any text, notification or feature.
12. Never send health data to a service hosted outside Europe. Check where a service stores data before adding it.

## Memory
Corrections that should hold for everyone become rules here, under Corrections, with why, the ticket and a review date. Decisions go in tickets. Personal preferences stay in each person's own tool memory. Never store ticket status, keys or tokens in any of them.

## Corrections
<!-- Each as: the rule — Why: … — Source: ticket, date — Review by: date. -->
