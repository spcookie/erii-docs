# Configuring Plugins

## Via CLI

```bash
# Read a plugin configuration value
erii config plugin get <plugin-name> <key>

# Set a plugin configuration value
erii config plugin set <plugin-name> <key> <value>
```

## Via Configuration Files

Plugin JSON configuration files are located at `plugins/config/<plugin-id>.json`:

```json
{
  "api_key": "your-key",
  "max_retries": 3,
  "timeout": 30
}
```

After editing the file directly, run:

```bash
erii reload
erii refresh
```

`reload` merges and refreshes local plugin configuration files, while `refresh` asks the running backend to reread its configuration cache.

??? info "Configuration priority (lowest to highest)"
    1. Default `plugin.json` in the plugin classpath
    2. `plugins/config/<plugin-id>.json` (overrides defaults)
    3. Custom path pointed to by system property `plugin.<plugin-id>.config` (highest priority)

## Plugin Enable/Disable

Control at Bot granularity in `application.conf`:

```hocon
onebot {
  bots {
    bot1 {
      enabled-plugins: "speech, net-ease-music"  # Only enable specified plugins
      # or
      disabled-plugins: "rollpig"                 # Disable specified plugins
    }
  }
}
```

- When not configured, all non-`builtin_` prefixed plugins are enabled by default
- `builtin_` prefixed plugins are always enabled and cannot be disabled
- When both `enabled-plugins` and `disabled-plugins` are present, `enabled-plugins` takes precedence
