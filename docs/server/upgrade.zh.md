# 升级与卸载

## 升级

```bash
npm update erii-bot
npx erii reload
```

`erii reload` 会自动合并新版配置文件，保留你已修改的设置。

如果升级包含较大变更，建议重启服务：

```bash
erii server restart
```

## 卸载

=== "删除项目目录"

    如果是用 `npm create erii-bot` 创建的项目，直接删除项目目录即可（`erii` 为默认目录名，请替换为你实际创建的目录名）：

    ```bash
    rm -rf erii
    ```

=== "npm 卸载"

    ```bash
    npm uninstall -g erii-bot
    # 或项目内
    npm uninstall erii-bot
    ```

卸载后以下数据会保留在项目目录中（如不再需要可手动删除）：

- `store/` — 数据库文件
- `logs/` — 日志文件
