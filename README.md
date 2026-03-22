# burrow-claude-plugin

Claude Code agents and commands specialized for the [burrow](https://github.com/oliverandrich/burrow) Go web framework.

## Commands

Interactive workflows that run in your main conversation. They ask questions, gather context, and delegate heavy work to the specialized agents below.

### `/burrow-feature-dev`

Guided feature development — discovery, codebase exploration, clarifying questions, architecture design, TDD implementation, review, and documentation.

```
/burrow-feature-dev Build a new contrib app for notifications with email and in-app delivery
```

### `/burrow-review`

Interactive code review against burrow conventions. Asks about scope, launches the reviewer agent, then helps you fix findings.

```
/burrow-review staged
/burrow-review contrib/jobs
```

### `/burrow-architect`

Interactive architecture design session. Asks clarifying questions, researches existing patterns, produces a blueprint, and persists it as a bean.

```
/burrow-architect How should I structure a webhook delivery system with retry logic?
```

## Agents

Specialized subagents with focused system prompts and tool restrictions. Used by the commands above, or directly via `@agent-name`.

### `burrow-dev`

Full-stack feature developer. Research, plan, TDD implementation, verify, document, close. Tracks work with beans.

### `burrow-architect`

Read-only architecture advisor. Designs implementation blueprints, persists them as beans.

### `burrow-reviewer`

Read-only code reviewer. Checks changes against burrow conventions and reports violations by severity.

### `burrow-user`

Simulates a first-time burrow developer. Reads documentation step by step and flags where it gets stuck, confused, or has to guess.

```
@burrow-user Read the quick start guide and tell me where you get stuck
```

## Installation

First, add the marketplace (one-time setup):

```bash
/plugin marketplace add oliverandrich/burrow-claude-plugin
```

Then install the plugin:

```bash
# Available in all your projects
/plugin install burrow-claude-plugin --scope user

# Or just for the current project
/plugin install burrow-claude-plugin --scope project
```

## What the agents know

All agents share deep knowledge of burrow's architecture:

- App lifecycle and boot sequence
- All optional interfaces (`Migratable`, `HasRoutes`, `Configurable`, etc.)
- Handler patterns (SSR, JSON API, HTMX row actions, file serving)
- Context helper naming conventions
- Configuration flag naming (`{appname}-{property}`)
- Repository pattern with Bun/SQLite
- Model struct tag conventions
- Renderer interface pattern
- Template namespacing and function registration
- Migration embedding pattern
- Testing with real SQLite (no mocks)
- Work tracking with beans

## Requirements

- [Claude Code](https://claude.ai/code) CLI
- A project using the [burrow](https://github.com/oliverandrich/burrow) framework
- [beans](https://github.com/hmans/beans) issue tracker (optional, for work tracking)
