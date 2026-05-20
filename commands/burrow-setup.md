---
description: Set up a burrow project for optimal Claude Code integration — detects contrib apps and configures CLAUDE.md
argument-hint: (no arguments needed)
---

# Burrow Project Setup

You are configuring this project for optimal use with the burrow Claude Code plugin. This command detects which burrow features are in use and adds guidance to the project's CLAUDE.md so that Claude Code automatically uses the right agents and follows the right patterns.

## Step 1: Detect Project Type

**Actions**:
1. Read `go.mod` to confirm burrow is imported and check the version
2. If burrow is NOT in `go.mod`, tell the user: "This doesn't appear to be a burrow project. Are you sure you want to continue?"

---

## Step 2: Detect Contrib Apps

**Actions**:
1. Search the codebase for burrow contrib imports: `grep -r "contrib/" --include="*.go"` or use the Grep tool
2. Build a list of which contrib apps are in use. Common ones:
   - `contrib/auth` — WebAuthn / passkey authentication
   - `contrib/session` — cookie-based sessions
   - `contrib/csrf` — CSRF protection (token + htmx headers)
   - `contrib/messages` — flash messages
   - `contrib/htmx` — htmx asset + request/response helpers
   - `contrib/staticfiles` — static file serving with content-hashed URLs
   - `contrib/admin` — admin panel coordinator
   - `contrib/jobs` — SQLite-backed background job queue
   - `contrib/sse` — Server-Sent Events
   - `contrib/healthcheck` — `/healthz/live` + `/healthz/ready` endpoints
   - `contrib/humanize` — locale-aware template formatting (naturaltime, intcomma, ...)
   - `contrib/ratelimit`, `contrib/secure`, `contrib/authmail` — additional middleware/utilities
3. i18n is part of burrow's core (`github.com/oliverandrich/burrow/i18n`), not a contrib — apps contribute translation files via `HasTranslations`.
4. CSS stack: since v0.20 the recommended path is Tailwind v4 via the `burrow tailwind` sub-command of the bundled `cmd/burrow` CLI (v0.21+). The standalone `cmd/burrow-tailwind` shim was removed in v0.22. Check for `tailwind.css` at the project root and `cmd/burrow` as a Go tool in `go.mod`.
5. Check for htmx usage in templates (look for `hx-` attributes in `.html` files).
6. Check for Den usage (look for `den` imports).

---

## Step 3: Generate CLAUDE.md Section

**Actions**:
1. Check if a `CLAUDE.md` exists in the project root
2. If yes, read it and check if a "Burrow Project" section already exists
   - If it exists, ask the user: "A burrow section already exists. Overwrite or skip?"
3. Generate the section based on what was detected:

```markdown
## Burrow Project

This project uses the [burrow](https://burrow.readthedocs.io/) Go web framework{htmx_note}.

### Agent Usage

For best results, use the specialized burrow agents:

- **Feature planning/architecture**: `/burrow-architect` or `@burrow-architect`
- **Feature implementation (TDD)**: `/burrow-feature-dev` or `@burrow-dev`
- **Code review**: `/burrow-review` or `@burrow-reviewer`
- **Den/database questions**: `@burrow-den-expert`

### Stack

- **Framework**: burrow {version}
- **ODM**: den (object-document mapper)
- **Contrib apps**: {detected list}
{htmx_section}

### Key Conventions

- Handlers use `burrow.HandlerFunc` signature, registered via `burrow.Handle(fn)`
- Context helpers: getter = short noun, setter = `WithX(ctx, val)`
- Repository: concrete struct with `*den.DB`, instantiated in `Configure()`
- Config flags: `{appname}-{property}` kebab-case
- Testing: `burrowtest.DB(t)`, testify, real SQLite
{htmx_conventions}
```

Where:
- `{htmx_note}` = " with htmx" if htmx is detected, else ""
- `{version}` = version from go.mod
- `{detected list}` = comma-separated contrib apps found
- `{htmx_section}` = if htmx detected, add:
  ```
  - **Frontend**: htmx (hypermedia-driven, no SPA)
  ```
- `{htmx_conventions}` = if htmx detected, add:
  ```
  - HTMX handlers: check `htmx.IsHTMX(r)` for partial vs full render
  - Redirects: always `htmx.SmartRedirect(w, r, url)`, never raw `http.Redirect`
  - CSRF: templates with `hx-post/put/patch/delete` MUST include `{{ csrfHxHeaders }}`
  - SSE handlers: register directly on mux, NOT wrapped in `burrow.Handle()`
  - Navigation: use `hx-boost="true"` on link containers
  ```

---

## Step 4: Write and Confirm

**Actions**:
1. If no `CLAUDE.md` exists, create one with the generated section
2. If `CLAUDE.md` exists, append the section (or replace existing burrow section)
3. Show the user what was written
4. Tell them: "Setup complete. Claude Code will now automatically use burrow agents and conventions for this project."

---
