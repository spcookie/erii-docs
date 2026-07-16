# reload / refresh / plugin refresh

These commands are all related to refreshing, but they operate at different layers:

| Command | Scope | Unload/reload plugins |
| --- | --- | --- |
| `erii reload` | Local CLI config files and plugin metadata | No |
| `erii refresh` | Runtime backend configuration cache | No |
| `erii plugin refresh [plugin-id]` | Runtime plugin lifecycle | Yes |

## erii reload

Rescan installed plugin packages and initialize or merge plugin configuration and metadata files.

```bash
erii reload
```

`reload` works on local files. It mainly:

- Merges new `application.conf`, `.env.local`, `.conf/*.json`, and related config templates
- Initializes or merges plugin `plugin.json` and `schema.json`
- Preserves user-modified values and only adds new fields
- Reloads CLI-side metadata schemas

Use it when:

- Plugins were installed, upgraded, or removed
- erii was upgraded and new config templates need to be merged
- Plugin config files were edited manually and local metadata should be refreshed

!!! note
    `erii reload` does not notify the running backend to reread configuration, and it does not unload or reload PF4J plugins. After changing configuration files, you usually also need `erii refresh`.

## erii refresh

Notify the running backend to refresh its configuration cache.

```bash
erii refresh
```

`refresh` calls the backend API:

```http
POST /api/config/refresh
```

The backend rereads application configuration and refreshes runtime caches for:

- `ConfigHolder`
- Bot roles
- Rule configuration
- MCP configuration
- OneBot connection diff, including added, removed, reconnected, role-updated, and failed bot keys

The CLI prints the counts returned by the backend, for example:

```text
Config refresh  OK
  Message       config refreshed

Summary
  Status        refreshed
  Target        backend config cache

Reloaded
  Config        true
  Roles         3
  Rules         7
  MCP           2

Bots
  Added         1 bot-a
  Removed       0
  Reconnected   0
  Role updated  0
  Failed        0
```

Use it when:

- API keys, model settings, pricing, or other application config changed
- `application.conf` or `.env.local` changed
- `erii reload` was run and the running backend should pick up the latest config
- OneBot bot config changed and the backend should add, remove, or reconnect bots based on the diff

!!! note
    `erii refresh` only refreshes configuration caches and connection state. It does not unload and reload plugin code. Use `erii plugin refresh` or restart the server for plugin code, dependency, or lifecycle changes.

## erii plugin refresh

Unload and reload running plugins.

```bash
erii plugin refresh
```

Without arguments, this refreshes all plugins. The refresh closes current extension contexts, clears route, command, and toolset registries, stops and unloads PF4J plugins, then loads, starts, and registers extensions again.

You can also refresh a single plugin:

```bash
erii plugin refresh <plugin-id>
```

For example:

```bash
erii plugin refresh official-qq-adapter
```

The CLI prints the refresh result and failure details, for example:

```text
Plugin refresh  ERROR HTTP 500
  Message       plugin refresh completed with failures

Summary
  Loaded        16 extensions
  Refreshed     11 plugins
  Failed        1 plugins

Failed plugins
  official-qq-adapter_GeneratedPassive_default
    Address already in use
```

Use it when:

- A plugin jar/zip was updated and you want to reload it without restarting the server
- A plugin added, removed, or changed `RouteExtension`, `CmdExtension`, toolsets, or other extensions
- A plugin owns lifecycle resources such as WebSockets, scheduled jobs, event subscriptions, KV storage, or vector storage and must run `onUnload()` before `onLoad()`
- You want to verify a single plugin's unload/load lifecycle

!!! warning
    Refreshing by `plugin-id` only applies to external PF4J plugins. Built-in extensions are rebuilt during full refresh and cannot be refreshed individually.

## Common workflows

After editing configuration files:

```bash
erii reload
erii refresh
```

After updating plugin packages:

```bash
erii reload
erii plugin refresh
erii refresh
```

After updating one plugin:

```bash
erii reload
erii plugin refresh <plugin-id>
erii refresh
```

## Changes that still require a restart

The following changes generally still require restarting the server:

- Database paths, connection pools, or other low-level storage configuration
- JVM options, classpath, or service startup parameters
- Binary-incompatible plugin dependency changes that cannot be safely unloaded/reloaded by the classloader
- Backend infrastructure component changes rather than plugin or regular config changes
