# Upgrading

Upgrade to the latest version:

```bash
# Upgrade erii
npm update erii-bot

# Reload configuration and plugins
npx erii reload
```

??? tip "After upgrading"
    It is recommended to run `erii reload` after upgrading to reload all plugin configurations and refresh the configuration cache. If the upgrade involves database schema changes, you will need to restart the server.

# Uninstalling

=== "Global Uninstall"

    ```bash
    npm uninstall -g erii-bot
    ```

=== "Local Project Uninstall"

    ```bash
    npm uninstall erii-bot
    ```

=== "Full Cleanup"

    If the project was created with `npm create erii-bot`, simply delete the project directory (`erii` is the default name; replace with your actual directory name):

    ```bash
    rm -rf erii
    ```

After uninstalling, the following data will remain in the project directory (delete manually if no longer needed):

- `store/` — Database files
- `logs/` — Log files
