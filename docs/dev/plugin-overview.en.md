# Plugin Development Overview

erii plugins are based on the PF4J framework and built using the `uesugi.erii-plugin` Gradle convention plugin.

## Extension Interfaces

Plugins support four extension interfaces:

| Interface | Description |
|:---|:---|
| `AgentExtension` | General-purpose extension, always participates in the Agent decision loop |
| `RouteExtension` | Route extension, matched and invoked after LLM intent classification |
| `CmdExtension` | Command extension, matches `/xxx` format user commands |
| `PassiveExtension` | Passive extension, background tasks and event listeners |

## PluginContext Capabilities

Plugins gain access to rich system capabilities through `PluginContext`:

| Capability | Description |
|:---|:---|
| `mem` | In-memory cache (supports expiration policies) |
| `kv` | Key-value persistent storage |
| `blob` | File/binary storage |
| `vector` | Vector embedding and semantic retrieval |
| `config` | Plugin configuration reading |
| `database` | Database queries (chat history) |
| `scheduler` | Task scheduler (cron / interval / delay) |
| `llm` | LLM invocation |
| `http` | HTTP client |
| `server` | Register HTTP routes |

## Development Modes

erii supports two plugin development modes:

- [**Class-based Development**](plugin-class.md) -- Suitable for complex plugins, providing fine-grained control over lifecycle and extension behavior
- [**Annotation-based Development**](plugin-annotation.md) -- Suitable for simple plugins, declarative feature definition with KSP auto-generated code
