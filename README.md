# burrow-claude-plugin

Claude Code agents specialized for the [burrow](https://github.com/oliverandrich/burrow) Go web framework.

## Agents

### `burrow-dev`

Full-stack feature developer. Research → Plan → TDD Implementation → Verify → Document → Close. Tracks work with beans, follows all burrow conventions, and ensures documentation is complete before finishing.

```
@burrow-dev Build a new contrib app for notifications with email and in-app delivery
```

### `burrow-architect`

Read-only architecture advisor. Designs implementation blueprints for new features and contrib apps, persists them as beans for later pickup by a developer.

```
@burrow-architect How should I structure a webhook delivery system with retry logic?
```

### `burrow-reviewer`

Read-only code reviewer. Checks changes against burrow conventions (handler patterns, context helpers, flag naming, repository patterns, templates, etc.) and reports violations by severity.

```
@burrow-reviewer Review my changes
```

### `burrow-user`

Simulates a developer using burrow for the first time. Reads documentation and mentally follows every step, flagging where it gets stuck, confused, or has to guess. Use to test docs, tutorials, and guides for clarity.

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

All three agents share deep knowledge of burrow's architecture:

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
- Testing with real in-memory SQLite (no mocks)
- Work tracking with beans

## Requirements

- [Claude Code](https://claude.ai/code) CLI
- A project using the [burrow](https://github.com/oliverandrich/burrow) framework
- [beans](https://github.com/hmans/beans) issue tracker (optional, for work tracking)
