# reload / refresh / plugin refresh

这三个命令都和“刷新”有关，但作用层级不同：

| 命令 | 作用范围 | 是否触发插件卸载/重载 |
| --- | --- | --- |
| `erii reload` | CLI 本地配置文件与插件元数据 | 否 |
| `erii refresh` | 正在运行的后端配置缓存 | 否 |
| `erii plugin refresh [plugin-id]` | 正在运行的后端插件生命周期 | 是 |

## erii reload

重新扫描已安装插件包，初始化或合并插件配置与元数据文件。

```bash
erii reload
```

`reload` 发生在 CLI 本地文件层面，主要做这些事：

- 合并新版 `application.conf`、`.env.local`、`.conf/*.json` 等配置模板
- 初始化或合并插件的 `plugin.json` 与 `schema.json`
- 保留用户已经修改过的配置值，只补充新增字段
- 重新加载 CLI 侧的元数据 schema

适合在以下场景使用：

- 安装、升级或卸载插件之后
- 升级 erii 版本之后，需要补齐新版配置模板
- 手动修改插件配置文件，希望重新整理本地配置元数据

!!! note
    `erii reload` 不会通知正在运行的后端重新读取配置，也不会卸载或重新加载 PF4J 插件。通常修改配置后还需要执行 `erii refresh`。

## erii refresh

通知正在运行的后端刷新配置缓存。

```bash
erii refresh
```

`refresh` 会调用后端接口：

```http
POST /api/config/refresh
```

后端会重新读取应用配置，并刷新以下运行时缓存：

- `ConfigHolder`
- Bot role
- rule 配置
- MCP 配置
- OneBot 连接差异，包括新增、移除、重连、角色更新和失败列表

CLI 会展示后端返回的统计信息，例如：

```text
Config refresh  OK
  Message       config refreshed

Summary
  Status        refreshed
  Target        backend config cache

Reloaded
  Config        true
  Roles         3
  Rules         7
  MCP           2

Bots
  Added         1 bot-a
  Removed       0
  Reconnected   0
  Role updated  0
  Failed        0
```

适合在以下场景使用：

- 修改 API Key、模型配置、价格配置等应用配置之后
- 修改 `application.conf` 或 `.env.local` 之后
- 执行 `erii reload` 后，需要让正在运行的后端读取最新配置
- 修改 OneBot bot 配置后，希望后端根据差异新增、移除或重连连接

!!! note
    `erii refresh` 只刷新配置缓存和连接状态，不会卸载并重新加载插件代码。插件代码、插件依赖或插件生命周期资源变更，需要使用 `erii plugin refresh` 或重启服务。

## erii plugin refresh

卸载并重新加载正在运行的插件。

```bash
erii plugin refresh
```

不带参数时刷新全部插件。刷新过程会关闭当前插件 extension context，清理路由、命令、工具集注册表，停止并卸载 PF4J 插件，然后重新加载、启动并注册扩展。

也可以只刷新指定插件：

```bash
erii plugin refresh <plugin-id>
```

例如：

```bash
erii plugin refresh official-qq-adapter
```

CLI 会展示刷新结果和失败详情，例如：

```text
Plugin refresh  ERROR HTTP 500
  Message       plugin refresh completed with failures

Summary
  Loaded        16 extensions
  Refreshed     11 plugins
  Failed        1 plugins

Failed plugins
  official-qq-adapter_GeneratedPassive_default
    Address already in use
```

适合在以下场景使用：

- 插件 jar/zip 更新后，希望不重启服务就重新加载插件
- 插件新增、删除或修改了 `RouteExtension`、`CmdExtension`、toolset 等扩展
- 插件持有 WebSocket、定时任务、事件订阅、KV/向量存储等生命周期资源，需要先 `onUnload()` 再 `onLoad()`
- 只想验证某个插件的卸载/加载流程

!!! warning
    指定刷新只适用于外部 PF4J 插件。内置扩展随全量刷新一起重建，不支持通过 `plugin-id` 单独刷新。

## 常见使用顺序

修改配置文件后：

```bash
erii reload
erii refresh
```

更新插件包后：

```bash
erii reload
erii plugin refresh
erii refresh
```

如果只更新了某个插件：

```bash
erii reload
erii plugin refresh <plugin-id>
erii refresh
```

## 需要重启服务的情况

以下变更通常仍建议重启服务：

- 修改数据库路径、连接池等底层存储配置
- 修改 JVM 参数、classpath 或服务启动参数
- 插件依赖发生二进制不兼容，导致类加载器无法安全卸载/重载
- 后端基础设施组件自身变更，而不是插件或普通配置变更
