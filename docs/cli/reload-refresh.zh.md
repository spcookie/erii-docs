# erii reload

重新加载插件配置和元数据，会自动合并新版配置文件模板，保留你已修改过的设置。

```bash
erii reload
```

## 什么时候需要 reload

- 安装或卸载插件之后
- 手动修改插件配置文件之后
- 升级 erii 版本之后

---

# erii refresh

通知正在运行的服务端刷新配置缓存。

```bash
erii refresh
```

## 什么时候需要 refresh

- 修改了 API Key 或模型配置之后
- 修改了 `application.conf` 或 `.env.local` 之后

## 不支持热刷新的配置

以下配置变更无法通过 `refresh` 生效，需要重启服务：

- **新增或移除 Bot**：`onebot.bots` 下 Bot 的增减需要重启
- **修改群组启用列表**：`groups.enable-groups` 变更需要重启
- **修改数据库相关配置**：数据库路径、连接池等配置需要重启

与 `reload` 的区别：`reload` 侧重插件配置的加载与合并，`refresh` 侧重应用配置的运行时热更新。
