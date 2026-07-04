# `erii config`

Interactive TUI configuration editor.

```bash
# Open TUI interactive editor
erii config

# Read/write main configuration
erii config app get <key>
erii config app set <key> <value>

# Read/write environment variables
erii config env get <key>
erii config env set <key> <value>

# Read/write plugin configuration
erii config plugin get <plugin> <key>
erii config plugin set <plugin> <key> <value>
```

## TUI Editor Panels

After entering `erii config`, use arrow keys to navigate between different configuration areas:

| Panel | Description |
|:---|:---|
| `env` | Environment variables (`.env.local`) |
| `app` | Application configuration (`application.conf`) |
| `plugin` | Plugin JSON configuration |
| `souls` | Bot persona configuration |
| `rules` | Rule files |
| `mcp` | MCP server configuration |
