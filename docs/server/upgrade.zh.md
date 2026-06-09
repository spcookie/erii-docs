# 升级与卸载

## 升级

```bash
npm update @spcookie/erii
npx erii reload
```

`erii reload` 会自动合并新版配置文件，保留你已修改的设置。

如果升级包含较大变更，建议重启服务：

```bash
erii server restart
```

## 卸载

=== "删除项目目录"

    如果是用 `npm create @spcookie/erii` 创建的项目，直接删除整个目录即可：

    ```bash
    rm -rf erii
    ```

=== "npm 卸载"

    ```bash
    npm uninstall -g @spcookie/erii
    # 或项目内
    npm uninstall @spcookie/erii
    ```

卸载后以下数据会保留在项目目录中（如不再需要可手动删除）：

- `store/` — 数据库文件
- `logs/` — 日志文件
- `plugins/` — 插件文件
