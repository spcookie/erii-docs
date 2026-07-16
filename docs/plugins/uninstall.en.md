# Uninstalling Plugins

=== "npm Uninstall"

    ```bash
    npm uninstall @some-author/erii-plugin-xxx
    ```

=== "Manual Uninstall"

    Delete the corresponding plugin files from the `plugins/` directory:

    ```bash
    rm /path/to/erii/plugins/my-plugin-*.zip
    rm -rf /path/to/erii/plugins/config/my-plugin.json
    ```

After uninstalling, run:

```bash
erii reload
erii plugin refresh
erii refresh
```

If the server is not running, the change will take effect on the next startup.
