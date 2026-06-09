# erii reload

重新加载插件配置和元数据。

```bash
erii reload
```

## 什么时候需要 reload

- 安装或卸载插件之后
- 手动修改插件配置文件之后
- 升级 erii 版本之后

`reload` 会自动合并新版配置文件模板，保留你已修改过的设置。

---

# erii refresh

通知正在运行的服务端刷新配置缓存。

```bash
erii refresh
```

## 什么时候需要 refresh

- 修改了 API Key 或模型配置之后
- 修改了 `application.conf` 或 `.env.local` 之后

与 `reload` 不同：`reload` 侧重插件配置，`refresh` 侧重应用配置的热更新。
