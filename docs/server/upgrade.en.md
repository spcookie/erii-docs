# Upgrading

Upgrade to the latest version:

```bash
# Upgrade erii
npm update @spcookie/erii

# Reload configuration and plugins
npx erii reload
```

??? tip "After upgrading"
    It is recommended to run `erii reload` after upgrading to reload all plugin configurations and refresh the configuration cache. If the upgrade involves database schema changes, you will need to restart the server.

# Uninstalling

=== "Global Uninstall"

    ```bash
    npm uninstall -g @spcookie/erii
    ```

=== "Local Project Uninstall"

    ```bash
    npm uninstall @spcookie/erii
    ```

=== "Full Cleanup"

    If the project was created with `npm create @spcookie/erii`, simply delete the entire project directory:

    ```bash
    rm -rf erii
    ```
