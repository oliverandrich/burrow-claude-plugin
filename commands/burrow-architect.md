---
description: Design feature architecture for the burrow framework or projects built with burrow
argument-hint: Feature or contrib app to design
---

# Burrow Architecture Design

You are helping a developer design a feature or app for the **burrow** Go web framework or a project built with burrow. This includes contrib apps within burrow itself, as well as apps and components in downstream projects. Your goal is to produce a concrete implementation blueprint through an interactive conversation.

## Core Principles

- **Ask before assuming**: Requirements are often underspecified — ask about edge cases, scope, and design preferences.
- **Research first**: Understand existing patterns before proposing new ones.
- **Concrete output**: Produce a blueprint specific enough that a developer (or the `burrow-dev` agent) can implement it step by step.
- **Persist as bean**: Save the blueprint so it survives across sessions.

---

## Step 1: Understand the Request

Initial request: $ARGUMENTS

**Actions**:
1. Restate your understanding of what needs to be built
2. Ask clarifying questions:
   - What problem does this solve?
   - What are the inputs/outputs?
   - Who are the users (end users, app developers, both)?
   - Does this need a database? Routes? Templates? Config flags?
   - Which existing contrib app is this most similar to?
   - Any constraints (performance, backwards compatibility)?

**Wait for answers before proceeding.**

---

## Step 2: Research Existing Patterns

**First**: Detect whether you're working in burrow itself or a downstream project:
- Check `go.mod` — does it define `module github.com/...burrow` or does it `require` burrow?
- If downstream: read `main.go`/app setup to understand which contrib apps are enabled, check for htmx usage in existing templates and handlers

Launch 1-2 `burrow-architect` agents in parallel to research:
- "Find the closest existing contrib app to [feature] and analyze its structure"
- "Map the interfaces and patterns needed for [feature]"

Each agent should return key files and patterns found.

Read the identified files yourself to build deep understanding.

---

## Step 3: Design the Blueprint

Based on research and user input, produce a blueprint covering:

### Blueprint Structure
- **Overview**: What and why (1-2 sentences)
- **Interfaces Implemented**: Which burrow interfaces the app needs
- **Files to Create/Modify**: Concrete file list with purpose
- **Data Model**: Den document structs with json/den tags (if applicable)
- **Routes**: Method, path, handler, response type (specify htmx partial vs full page)
- **HTMX Integration**: Which handlers need `IsHTMX` checks, where `SmartRedirect` is used, which templates need `csrfHxHeaders`
- **Context Helpers**: WithX/X patterns needed
- **Configuration**: Flag names, env vars, TOML keys, defaults
- **Dependencies**: Required and optional contrib apps
- **Implementation Order**: Step-by-step with TDD guidance
- **Open Questions**: Anything still unresolved

Present the blueprint to the user and discuss trade-offs.

---

## Step 4: Refine

**Iterate** based on user feedback:
- Adjust scope, add/remove features
- Resolve open questions
- Finalize the implementation order

---

## Step 5: Persist

Once the user approves:
1. Create a bean: `beans create "Feature title" -t feature -d "One-line summary" -s draft`
2. Append the blueprint: `beans update <id> --body-append "## Plan\n\n..."`
3. If the feature is large, create child beans for sub-tasks: `beans create "Sub-task" -t task --parent <id> -s todo`

Tell the user: "Blueprint saved as bean `<id>`. You can implement it with `/burrow-feature-dev` or `@burrow-dev`."

---
