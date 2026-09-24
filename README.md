# research-plan-feedback

A Claude Code plugin that packages a `research → plan → feedback → add-todos → implement` workflow as five skills, inspired by [Boris Tane's "How I use Claude Code"](https://boristane.com/blog/how-i-use-claude-code/).

The idea: separate *understanding*, *planning*, and *doing* into distinct phases with a durable, human-reviewable artifact (`research.md`, `plan.md`) between each one, instead of asking Claude to research-plan-and-implement in a single shot.

## Workflow

1. **`/research <subjects to study>`** — Deep-reads the given files/folders/systems and writes findings to `research.md` at the project root. Read-only: no code or plan changes.
2. **`/plan <feature or change description>`** — Reads `research.md` (if present) and the relevant codebase, then writes a detailed implementation plan to `plan.md` at the project root, with real file paths, code snippets, and trade-offs. Does not implement anything.
3. Review `plan.md` yourself and drop inline notes/annotations anywhere you disagree (e.g. quoted asides like `"skip this, we already have a helper for it"`).
4. **`/feedback`** — Reads your annotations in `plan.md`, applies the corrections throughout the plan, and cleans up the leftover annotation text. Does not implement anything.
5. **`/add-todos`** — Appends a granular `## Todo` checklist to `plan.md`, broken down by phase. Does not implement anything.
6. **`/implement`** — Executes `plan.md` (and its todo list), checking off tasks as it completes them.

Repeat steps 3–4 as many times as you like before moving to todos/implementation — the plan document is the single source of truth, and each phase only touches it or the codebase, never both at once.

### Artifacts stay out of git automatically

`research.md` and `plan.md` are scratch documents, not something you want to commit or manually gitignore per project. Both `/research` and `/plan` add themselves to the repo's `.git/info/exclude` on first run (one approval, since `.git/` is a protected path) — git's built-in per-clone ignore list. It behaves like `.gitignore` but lives inside `.git/`, so it's never committed and never visible to collaborators, and it doesn't require a global gitignore rule that follows you into every repo.

`/research` also deletes any existing `research.md` and `plan.md` before starting (again, one approval), so you never start a new research pass with a stale report (or a stale plan from an unrelated prior task) still sitting in the working tree. Since both files are git-excluded, this delete is **not** recoverable through git — if you want to keep a plan around, copy it out (or rename it) before running `/research` again.

### Bare vs. namespaced commands

Plugin skills are always registered under a namespaced form, `/research-plan-feedback:research`. Claude Code also exposes the bare form (`/research`) as a shorthand automatically, as long as no other installed skill or command already claims that name. If a name collision ever comes up (another plugin also defining `/research`, say), fall back to the namespaced form to target this plugin's version explicitly.

## Install

```
/plugin marketplace add tai2/research-plan-feedback-plugin
/plugin install research-plan-feedback@research-plan-feedback
```

Then run `/reload-plugins` if you installed mid-session. Skills are available as both `/research-plan-feedback:research` and the bare `/research` (see [Bare vs. namespaced commands](#bare-vs-namespaced-commands)).

## Local development

```
claude --plugin-dir ./research-plan-feedback-plugin
```

## Skills

| Skill | Forked context | Allowed tools | Writes to |
|---|---|---|---|
| `research` | yes | Read, Glob, Grep, Edit, Write, `git rev-parse`, `git log`, `git blame` | `research.md` |
| `plan` | yes | Read, Glob, Grep, Edit, Write, `git rev-parse`, `git log` | `plan.md` |
| `feedback` | yes | Read, Glob, Grep, Edit, `git rev-parse` | `plan.md` |
| `add-todos` | yes | Read, Edit, `git rev-parse` | `plan.md` |
| `implement` | no | Read, Write, Edit, Glob, Grep, `pnpm typecheck`, `git rev-parse`, `git status`, `git diff`, `git log` | your codebase |

`research`, `plan`, `feedback`, and `add-todos` run in a forked context so they don't pollute your main conversation with the full contents of files they read — only the resulting document changes surface back. `implement` runs in the main context since it needs to carry that context through a longer, stateful session.

`implement`'s default `allowed-tools` pre-approves `pnpm typecheck` — adjust it if your project uses a different package manager. `allowed-tools` only *pre-approves*; it doesn't restrict, so anything not listed (a `git commit`, another package-manager script) still runs through your normal permission rules.
