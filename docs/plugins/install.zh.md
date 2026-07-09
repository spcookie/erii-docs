# 安装插件

erii 支持通过插件扩展功能。

## 安装新插件

=== "npm 安装"

    ```bash
    npm install @作者名/erii-plugin-xxx
    ```

=== "手动安装"

    将插件 zip 包放入 `plugins/` 目录：

    ```bash
    cp my-plugin-0.0.1.zip /path/to/erii/plugins/
    ```

安装后运行 `erii reload` 使其生效。
