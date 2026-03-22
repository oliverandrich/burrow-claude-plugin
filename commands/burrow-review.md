---
description: Review code changes against burrow conventions — works in burrow itself and projects built with burrow
argument-hint: Optional scope (e.g., "staged", "contrib/jobs", or a commit range)
---

# Burrow Code Review

You are helping a developer review their code changes against **burrow** framework conventions before committing. This works both within the burrow framework itself and in downstream projects that use burrow.

## Core Principles

- **Interactive first**: Ask what to review before diving in.
- **Delegate the analysis**: Use the `burrow-reviewer` agent for the actual review.
- **Discuss findings**: Present results and help the user decide what to fix.

---

## Step 1: Determine Scope

Initial request: $ARGUMENTS

**If no scope is given**, ask the user:
- "What should I review? Options:"
  - Staged changes (`git diff --cached`)
  - All uncommitted changes (`git diff`)
  - Changes since a branch point (`git diff main...HEAD`)
  - A specific directory or file

**If a scope is given**, confirm it and proceed.

---

## Step 2: Quick Context

Before launching the reviewer, quickly check:
1. Run the appropriate `git diff` to see what changed
2. Briefly summarize the scope to the user: number of files, which packages, what kind of changes

Ask: "Ready to review, or do you want to adjust the scope?"

---

## Step 3: Review

Launch a `burrow-reviewer` agent with a clear task:
- Tell it exactly which files/diff to review
- Include any context from the conversation (e.g., "this is a new contrib app", "this is a bug fix in auth")

---

## Step 4: Discuss Findings

Present the reviewer's findings to the user:
1. **Must Fix** — convention violations, bugs
2. **Should Fix** — style issues, missing patterns
3. **Consider** — suggestions for improvement
4. **Clean** — files that follow all conventions

For each finding, ask: "Want me to fix this?"

If the user wants fixes, make them directly — no need to re-launch an agent for small changes.

---
