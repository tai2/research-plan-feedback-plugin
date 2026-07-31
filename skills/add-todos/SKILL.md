---
name: add-todos
description: Add a detailed todo checklist to the current plan with all phases and individual tasks
context: fork
allowed-tools: Read, Edit
---

The project root is: !`git rev-parse --show-toplevel`

Read the `plan.md` at the project root.

Add a `## Todo` section at the end of the plan with a detailed, granular checklist:

- Break down every phase into individual tasks using `- [ ]` checkbox format
- Group tasks by phase/step matching the plan's structure
- Each task should be specific and verifiable (not vague like "implement feature")
- Include tasks for: code changes, test updates, type checking, and any migration steps

**Do not implement anything. Only add the todo list to the plan.**
