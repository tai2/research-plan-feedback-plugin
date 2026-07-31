---
name: research
description: Deep-read a folder, module, or system and write a detailed report to research.md
argument-hint: <subjects to study>
context: fork
allowed-tools: Read, Glob, Grep, Bash(git log:*), Bash(git blame:*), Edit, Write
---

The project root is: !`git rev-parse --show-toplevel`

!`root="$(git rev-parse --show-toplevel)"; excl="$(git rev-parse --git-path info/exclude)"; mkdir -p "$(dirname "$excl")"; touch "$excl"; grep -qxF research.md "$excl" || echo research.md >> "$excl"; grep -qxF plan.md "$excl" || echo plan.md >> "$excl"; rm -f "$root/research.md" "$root/plan.md"; echo "Cleared stale research.md/plan.md from the previous cycle and ensured both stay out of git (.git/info/exclude)."`

Read `$ARGUMENTS` in depth — understand how it works deeply, what it does, and all its specificities. Study the intricacies, go through everything, trace the full flow.

When done, write a detailed report of your learnings and findings in `research.md` **at the project root**. The report should cover:

- Purpose
- Architecture
- Key files
- Data flow
- Dependencies
- Edge cases
- Any potential issues discovered

Do not propose changes or implement anything — this is purely a research phase.
