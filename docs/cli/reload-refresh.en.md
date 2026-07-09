# `erii reload`

Reload plugin configuration and metadata. Automatically merges new configuration templates while preserving
user-modified settings.

```bash
erii reload
```

## When to use `reload`

- After installing or uninstalling plugins
- After manually editing plugin configuration files
- After upgrading erii

---

# `erii refresh`

Notify the running server to refresh its configuration cache.

```bash
erii refresh
```

## When to use `refresh`

- After modifying API Key or model configuration
- After modifying `application.conf` or `.env.local`

## Config changes that require a restart

The following changes cannot be applied via `refresh` and require a server restart:

- **Adding or removing bots**: Changes under `onebot.bots` require a restart
- **Modifying group lists**: `groups.enable-groups` changes require a restart
- **Database configuration**: Database path, connection pool settings require a restart

`reload` focuses on plugin configuration loading and merging, while `refresh` handles runtime hot-reload of application
configuration.
