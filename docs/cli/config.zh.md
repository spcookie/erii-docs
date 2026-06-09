# erii config

配置编辑器，用于查看和修改 erii 的各项设置。

## 交互式编辑器

```bash
erii config
```

进入后可以用方向键浏览和编辑以下配置区：

| 配置区 | 说明 |
|:---|:---|
| `env` | 环境变量（API Key、端口等） |
| `app` | 应用配置（模型参数、连接地址等） |
| `plugin` | 插件配置 |
| `souls` | Bot 人格设定 |
| `rules` | 行为规则 |

## 命令行读写

也可以直接在命令行中读写配置：

```bash
# 读取应用配置
erii config app get llm.choice-provider

# 修改应用配置
erii config app set llm.choice-provider minimax

# 读取环境变量
erii config env get GOOGLE_API_KEY

# 修改环境变量
erii config env set ERII_PORT 9090

# 读取插件配置
erii config plugin get speech api_key

# 修改插件配置
erii config plugin set speech api_key "your-key"
```

配置键使用点分隔的路径，例如 `llm.google.api-key`。
