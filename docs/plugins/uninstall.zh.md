# 卸载插件

=== "npm 卸载"

    ```bash
    npm uninstall @作者名/erii-plugin-xxx
    ```

=== "手动卸载"

    删除 `plugins/` 目录下对应的插件文件：

    ```bash
    rm /path/to/erii/plugins/my-plugin-*.zip
    ```

卸载后运行 `erii reload` 使更改生效。
