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

安装后运行：

```bash
erii reload
erii plugin refresh
erii refresh
```

如果服务未运行，也可以直接在下次启动时加载插件。`plugin refresh` 用于运行时卸载并重新加载插件生命周期。
