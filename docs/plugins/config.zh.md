# 配置插件

## 通过命令行配置

```bash
# 读取配置
erii config plugin get <插件名> <配置项>

# 修改配置
erii config plugin set <插件名> <配置项> <值>
```

示例：

```bash
erii config plugin get speech api_key
erii config plugin set speech api_key "your-key"
```

## 通过文件配置

插件配置文件位于 `plugins/config/<插件id>.json`：

```json
{
  "api_key": "your-key",
  "max_retries": 3,
  "timeout": 30
}
```

直接编辑该文件后运行 `erii reload` 生效。

## 启用或禁用插件

在 `application.conf` 中按 Bot 控制：

```hocon
onebot {
  bots {
    bot1 {
      enabled-plugins: "speech, net-ease-music"   # 仅启用这两个
      # 或
      disabled-plugins: "rollpig"                  # 禁用这个
    }
  }
}
```

- 不配置时，所有插件默认启用
- `builtin_` 开头的插件始终启用
