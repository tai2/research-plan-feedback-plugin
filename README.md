# research-plan-feedback

A Claude Code plugin that packages a `research → plan → feedback → add-todos → implement` workflow as five skills, inspired by [Boris Tane's "How I use Claude Code"](https://boristane.com/blog/how-i-use-claude-code/).

The idea: separate *understanding*, *planning*, and *doing* into distinct phases with a durable, human-reviewable artifact (`research.md`, `plan.md`) between each one, instead of asking Claude to research-plan-and-implement in a single shot.

## Workflow

1. **`/research-plan-feedback:research <subjects to study>`** — Deep-reads the given files/folders/systems and writes findings to `research.md` at the project root. Read-only: no code or plan changes.
2. **`/research-plan-feedback:plan <feature or change description>`** — Reads `research.md` (if present) and the relevant codebase, then writes a detailed implementation plan to `plan.md` at the project root, with real file paths, code snippets, and trade-offs. Does not implement anything.
3. Review `plan.md` yourself and drop inline notes/annotations anywhere you disagree (e.g. quoted asides like `"skip this, we already have a helper for it"`).
4. **`/research-plan-feedback:feedback`** — Reads your annotations in `plan.md`, applies the corrections throughout the plan, and cleans up the leftover annotation text. Does not implement anything.
5. **`/research-plan-feedback:add-todos`** — Appends a granular `## Todo` checklist to `plan.md`, broken down by phase. Does not implement anything.
6. **`/research-plan-feedback:implement`** — Executes `plan.md` (and its todo list), checking off tasks as it completes them.

Repeat steps 3–4 as many times as you like before moving to todos/implementation — the plan document is the single source of truth, and each phase only touches it or the codebase, never both at once.

## Install

```
/plugin marketplace add tai2/research-plan-feedback-plugin
/plugin install research-plan-feedback@research-plan-feedback
```

Then run `/reload-plugins` if you installed mid-session.

## Local development

```
claude --plugin-dir ./research-plan-feedback-plugin
```

## Skills

| Skill | Forked context | Allowed tools | Writes to |
|---|---|---|---|
| `research` | yes | Read, Glob, Grep, `git log`, `git blame`, Edit, Write | `research.md` |
| `plan` | yes | Read, Glob, Grep, `git log`, `pnpm *`, Edit, Write | `plan.md` |
| `feedback` | yes | Read, Glob, Grep, Edit | `plan.md` |
| `add-todos` | yes | Read, Edit | `plan.md` |
| `implement` | no | Read, Write, Edit, Glob, Grep, `pnpm *`, `git *` | your codebase |

`research`, `plan`, `feedback`, and `add-todos` run in a forked context so they don't pollute your main conversation with the full contents of files they read — only the resulting document changes surface back. `implement` runs in the main context since it needs to carry that context through a longer, stateful session.

Some default `allowed-tools` reference `pnpm` — adjust the `plan.md`/`implement` skills if your project uses a different package manager.
