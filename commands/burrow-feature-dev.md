---
description: Guided feature development for the burrow framework or projects built with burrow — research, plan, implement with TDD
argument-hint: Optional feature description
---

# Burrow Feature Development

You are helping a developer implement a feature for the **burrow** Go web framework or a project built with burrow. This includes building new contrib apps within burrow itself, as well as building apps, handlers, templates, and other components in downstream projects that use burrow as a dependency.

Follow a systematic approach: understand the codebase, ask questions, design the architecture, then implement with TDD.

## Core Principles

- **Ask clarifying questions**: Identify ambiguities, edge cases, and underspecified behaviors. Ask early, before designing.
- **Understand before acting**: Read and comprehend existing code patterns first.
- **TDD**: Write failing tests first, then implement, then refactor.
- **Track with beans**: Use `beans` to track all work — never TodoWrite.

---

## Phase 1: Discovery

**Goal**: Understand what needs to be built

Initial request: $ARGUMENTS

**Actions**:
1. Check if a bean already exists: `beans list --json -S "search term"`
2. If not, create one: `beans create "Title" -t feature -d "Description" -s in-progress`
3. If the feature is unclear, ask the user:
   - What problem are they solving?
   - What should the feature do?
   - Any constraints or requirements?
4. Summarize understanding and confirm with user

---

## Phase 2: Codebase Exploration

**Goal**: Understand relevant existing code and patterns

**Actions**:
1. **Detect project type**: Check `go.mod` — are we in burrow itself or a downstream project?
   - If downstream: read app setup, check which contrib apps are enabled, examine existing htmx usage in templates/handlers
2. Launch 2-3 `burrow-architect` agents in parallel, each targeting a different aspect:
   - "Find existing contrib apps similar to [feature] and trace their implementation"
   - "Map the architecture and interfaces relevant to [feature area]"
   - "Analyze patterns for [specific aspect: handlers, templates, config, etc.]"
3. Each agent should return a list of 5-10 key files to read
4. Read all files identified by agents to build deep understanding
5. Append findings to the bean body under `## Research`

---

## Phase 3: Clarifying Questions

**Goal**: Fill in all gaps before designing

**CRITICAL**: Do NOT skip this phase.

**Actions**:
1. Review codebase findings and the original request
2. Identify underspecified aspects: edge cases, error handling, integration points, scope, design preferences, which burrow interfaces to implement
3. **Present all questions to the user in a clear, organized list**
4. **Wait for answers before proceeding**

If the user says "whatever you think is best", provide your recommendation and get explicit confirmation.

---

## Phase 4: Architecture Design

**Goal**: Design the implementation approach

**Actions**:
1. Launch a `burrow-architect` agent to produce a blueprint following burrow conventions
2. Review the blueprint — ensure it covers: interfaces, file layout, data model, routes, config flags, context helpers, implementation order
3. Present the blueprint to the user with your assessment
4. **Ask user to approve before implementing**
5. Append the approved plan to the bean body under `## Plan`

---

## Phase 5: Implementation

**Goal**: Build the feature with TDD

**DO NOT START WITHOUT USER APPROVAL**

**Actions**:
1. Wait for explicit approval
2. For each step in the implementation order:
   a. Write failing tests first
   b. Implement minimum code to make tests pass
   c. Refactor if needed
3. After each step, run verification:
   - `go build ./...`
   - `go test ./...`
   - `golangci-lint run ./...`
4. Update bean todo items as you progress

### HTMX Implementation Checklist

For every handler that renders HTML, verify:
- [ ] Checks `htmx.IsHTMX(r)` to decide partial vs full page render
- [ ] Uses `htmx.SmartRedirect(w, r, url)` instead of `http.Redirect`
- [ ] Templates with `hx-post/put/patch/delete` include `{{ csrfHxHeaders }}`
- [ ] Navigation links use `hx-boost="true"`
- [ ] Form handlers target specific elements via `hx-target`
- [ ] Validation errors re-render the form fragment, don't redirect
- [ ] SSE handlers are NOT wrapped in `burrow.Handle()`

---

## Phase 6: Quality Review

**Goal**: Ensure code follows all burrow conventions

**Actions**:
1. Launch a `burrow-reviewer` agent to review all changes against burrow conventions
2. Present findings to the user grouped by severity
3. **Ask user what to fix** (fix now, fix later, or proceed as-is)
4. Address issues based on user decision

---

## Phase 7: Documentation & Completion

**Goal**: Complete all integration points

**Actions**:
1. Update relevant documentation. What to update depends on whether you're working in burrow itself or a downstream project:
   - **In burrow**: `docs/contrib/<name>.md`, `mkdocs.yml` nav entry, `docs/llms.txt`, `CHANGELOG.md`, `CLAUDE.md` contrib list
   - **In downstream projects**: README, CHANGELOG, or whatever documentation conventions the project uses
2. Add `## Summary of Changes` to the bean
3. Mark bean as completed
4. Summarize: what was built, key decisions, files modified, suggested next steps

---
