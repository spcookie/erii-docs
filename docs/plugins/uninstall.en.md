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

After uninstalling, run `erii reload` for the changes to take effect.
