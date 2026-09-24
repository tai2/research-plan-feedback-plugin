---
name: plan
description: Write a detailed implementation plan document in plans/ based on the actual codebase
argument-hint: <feature or change description>
context: fork
effort: high
allowed-tools: Read, Glob, Grep, Edit, Write, Bash(git rev-parse:*), Bash(git log:*)
---

The project root is: !`git rev-parse --show-toplevel`

The git exclude file is: !`git rev-parse --path-format=absolute --git-path info/exclude`

## Before you start

Read the git exclude file (path above, used verbatim). If it is missing a `research.md` or a `plan.md` line, add the missing one(s) and change nothing else (create the file if it does not exist). It lives under `.git/`, so this write is never pre-approved — expect to approve it once per repo.

## Plan

Read `research.md` at the project root if it exists to build on prior research. Study the relevant parts of the codebase that relate to the following:

$ARGUMENTS

Write a detailed plan document at `plan.md` **at the project root**.

The plan must include:

- Goal section
- Architecture / approach explanation
- Code snippets showing the actual changes to make
- File paths that need modification
- Considerations and trade-offs

Base the plan on the actual codebase — reference real file paths, real function names, real patterns already in use.

**Do not include a todo list** — that is a separate phase handled by `/add-todos`.

**Do not implement anything.** Only produce the plan document.
