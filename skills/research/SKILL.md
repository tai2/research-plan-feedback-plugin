---
name: research
description: Deep-read a folder, module, or system and write a detailed report to research.md
argument-hint: <subjects to study>
context: fork
allowed-tools: Read, Glob, Grep, Edit, Write, Bash(git rev-parse:*), Bash(git log:*), Bash(git blame:*)
---

The project root is: !`git rev-parse --show-toplevel`

The git exclude file is: !`git rev-parse --path-format=absolute --git-path info/exclude`

## Before you start

Use the two paths above verbatim in anything you run — never a command whose target is a shell variable or a command substitution.

1. Read the git exclude file. If it is missing a `research.md` or a `plan.md` line, add the missing one(s) and change nothing else (create the file if it does not exist). It lives under `.git/`, so this write is never pre-approved — expect to approve it once per repo.
2. Delete a stale `research.md` and `plan.md` at the project root if they exist — leftovers from the previous cycle. Spell both paths out in full: `rm -f <project root>/research.md <project root>/plan.md`.

## Research

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
