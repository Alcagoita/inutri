# inutri — setup report

Set up with Brush Frame (step 4 of the framework) on 22 November 2026.

**Coming back to the project:** in the Claude desktop app, choose the folder `~/Projects/inutri` when starting a session. In a terminal: `cd ~/Projects/inutri` then `claude`.

## Checklist

- [x] Project folder `~/Projects/inutri`, Git with `main`
- [x] Repository https://github.com/Alcagoita/inutri (public)
- [x] Two lanes: `develop` (default) and `main`
- [x] Ruleset on `main` and `develop`: pull request required, checks `test` and `ticket-key` must pass, no direct push or deletion, only the owner merges (solo: 0 approvals)
- [x] Branches deleted after merge; secret scanning and push protection on
- [x] CI `test` job for Kotlin Multiplatform (Java 17 + Gradle, runs `./gradlew check` once a Gradle project exists)
- [x] Ticket key check (CI) and local `commit-msg` hook: every commit and PR title starts with `INU-12: …`
- [x] Tasks in Jira, project **INU**: https://olegarioncnascimento.atlassian.net/jira/software/projects/INU
- [x] Board with four columns: To do, In development, Testing, Done
- [x] Practice ticket INU-1 run end to end (branch, PR #3, checks, merge, Done)
- [x] Idea saved: [INU-2](https://olegarioncnascimento.atlassian.net/browse/INU-2)
- [x] Rule file `AGENTS.md` + `CLAUDE.md` (`@AGENTS.md`), and team summary `docs/visao-do-produto.md`
- [~] AI review (CodeRabbit) installed, but on this repository it reviews **only when asked** (fewer than 10 stars): tick **Trigger review** on the PR, or comment `@coderabbitai review`
- [ ] **Waiting on the owner:** "Won't do" resolution in Jira (needs a Jira admin): Settings → Issues → Resolutions → add **Won't do**
- [ ] **Optional:** Jira status still named "In Progress" (the column is "In development"); rename it in the board's ⋯ → Manage workflow if you want them to match
- [ ] **Waiting on the owner:** `kotlin-lsp` program not installed (Apple Command Line Tools too old for macOS 15). Update them in System Settings → General → Software Update, then `brew install JetBrains/utils/kotlin-lsp`

Changes after the first plan: tasks moved from GitHub Issues to Jira (PR #2; GitHub issue #1 closed as not planned), and the stack changed from React Native to Kotlin Multiplatform (PR #4).

## Skills and plugins

Installed for Claude Code: `kotlin-lsp`, `security-guidance` (health data). Not used: Pigment (can be added any time).

## Rule file

Created: `AGENTS.md` (product, promises, model facts, workflow, Gitflow, never-do rules, memory), `CLAUDE.md` importing it. Team-facing summary in Portuguese: `docs/visao-do-produto.md`.

## Beta testers

Later. Options when ready: Firebase App Distribution, Google Play internal testing, TestFlight. Store accounts need fees and identity checks.

## For developers joining

1. Install an AI tool (Claude Code or Codex).
2. Clone the repository: `gh repo clone Alcagoita/inutri`.
3. Run the Brush Frame setup skill in the clone: it finds the project already set up and only does what is personal to your computer (turns on the commit hook with `git config core.hooksPath .githooks`, Jira access, suggested skills).
4. Ask the owner for access to the Jira project INU.
5. When the team grows, the ruleset changes to require one approval on every pull request.
