# `erii reload`

Reload plugin configuration and metadata.

```bash
erii reload
```

!!! tip "Use cases"
    - After manually editing plugin JSON configuration files
    - After installing/uninstalling plugins
    - After updating plugin versions

# `erii refresh`

Refresh backend configuration cache. Unlike `reload`, `refresh` notifies the running server to re-read configuration files.

```bash
erii refresh
```

!!! tip "Use cases"
    - After modifying `application.conf` or `.env.local` without needing to restart the server
    - After changing LLM API Key or model configuration
