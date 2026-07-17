# Install erii-plugin-skill

`erii-plugin-skill` is an agent skill package for developing Erii plugins. It helps skill-aware agents create, modify, review, and scaffold standalone Erii plugin projects.

It is not an Erii runtime plugin. Do not place it in an Erii project's `plugins/` directory, and do not reload it with `erii plugin refresh`. Install it into your agent's skill environment so the agent can read the public SPI, annotation mode, `PluginContext`, `erii-common` APIs, and Gradle scaffolding conventions while working on Erii plugin code.

## When To Use It

- You have the Erii source tree locally and want the agent to follow the existing `erii-spi`, `erii-common`, `erii-plugin-gradle`, and example plugin patterns.
- You do not have the Erii source tree locally, but dependency artifacts are available and the agent needs source-free public API references through `llms.txt` and `references/`.
- You need to scaffold an annotation-mode or class-based PF4J/SPI plugin.
- You need the agent to choose between `@Cmd`, `@Route`, `@Passive`, `@LLMTool`, `PluginContext`, and `ArgParserHolder`.

## Prerequisites

- Node.js 18+
- An agent runtime that supports skill packages
- `npx`

## Install

The `--agent` value depends on your agent runtime. The command below uses `<agent>` as a placeholder:

```bash
npx skills add https://github.com/spcookie/erii-plugin-skill.git --global --agent <agent> --skill erii-plugin-skill --yes
```

For example, if your agent id is `codex`:

```bash
npx skills add https://github.com/spcookie/erii-plugin-skill.git --global --agent codex --skill erii-plugin-skill --yes
```

List installed skills:

```bash
npx skills list --global --agent <agent>
```

## Use It

Ask your skill-aware agent for an Erii plugin development task directly. For example:

```text
Create an Erii plugin with a /roll command using class-based PF4J/SPI mode.
```

Or:

```text
Review this Erii annotation plugin for correct @ChatMessage and useToolMeta() usage.
```

The agent reads `erii-plugin-skill/SKILL.md` and then opens references as needed:

- `llms.txt`: quick entry point for source-free environments.
- `references/erii-plugin-api.md`: public SPI, annotation API, `PluginContext`, scheduling, storage, and tool registration.
- `references/erii-common-api.md`: `erii-common` APIs supplied transitively by the plugin Gradle convention.
- `references/development-modes.md`: choosing annotation mode or class-based PF4J/SPI mode.
- `references/context-helpers.md`: annotation helper lifetime and coroutine context boundaries.
- `references/gradle-projects.md`: Gradle project layout, `plugin.json`, `schema.json`, and scaffold scripts.

## Difference From Erii Runtime Plugins

| Item | erii-plugin-skill | Erii runtime plugin |
|:---|:---|:---|
| Install location | Agent skill environment | Erii project `plugins/` directory or npm dependency |
| Purpose | Helps an agent develop plugins | Extends a running Erii bot |
| Install command | `npx skills add ...` | `npm install ...` or copy a plugin zip |
| Requires `erii plugin refresh` | No | Yes, when updating at runtime |

If you want to install a plugin that runs inside Erii, read [Installing Plugins](../plugins/install.md).

## Update

Run the install command again to fetch the latest version:

```bash
npx skills add https://github.com/spcookie/erii-plugin-skill.git --global --agent <agent> --skill erii-plugin-skill --yes
```
