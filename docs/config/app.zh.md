# 应用配置

应用配置通过 `erii config` 命令修改，无需手动编辑配置文件。

```bash
erii config                    # 打开 TUI 交互式编辑器
erii config app get <键>       # 读取配置项
erii config app set <键> <值>  # 修改配置项
```

---

## LLM 对话模型

### 通用设置

| 配置键 | 说明 | 可选值 |
|:---|:---|:---|
| `llm.choice-provider` | 默认 AI 提供商 | `google` / `deep-seek` / `minimax` / `openai` / `anthropic` / `openrouter` |

### Google Gemini

| 配置键 | 说明 |
|:---|:---|
| `llm.google.api-key` | API Key |
| `llm.google.base-url` | API 地址 |
| `llm.google.models.lite` | 轻量模型 |
| `llm.google.models.flash` | 快速模型 |
| `llm.google.models.pro` | 高质量模型 |

### DeepSeek

| 配置键 | 说明 |
|:---|:---|
| `llm.deep-seek.api-key` | API Key |
| `llm.deep-seek.base-url` | API 地址 |
| `llm.deep-seek.models.lite` | 轻量模型 |

### MiniMax

| 配置键 | 说明 |
|:---|:---|
| `llm.minimax.api-key` | API Key |
| `llm.minimax.base-url` | API 地址 |
| `llm.minimax.models.lite` | 轻量模型 |
| `llm.minimax.models.pro` | 高质量模型 |

### OpenAI

| 配置键 | 说明 |
|:---|:---|
| `llm.openai.api-key` | API Key |
| `llm.openai.base-url` | API 地址 |
| `llm.openai.models.lite` | 轻量模型 |
| `llm.openai.models.flash` | 快速模型 |
| `llm.openai.models.pro` | 高质量模型 |

### Anthropic

| 配置键 | 说明 |
|:---|:---|
| `llm.anthropic.api-key` | API Key |
| `llm.anthropic.base-url` | API 地址 |
| `llm.anthropic.models.lite` | 轻量模型 |
| `llm.anthropic.models.flash` | 快速模型 |

### OpenRouter

| 配置键 | 说明 |
|:---|:---|
| `llm.openrouter.api-key` | API Key |
| `llm.openrouter.base-url` | API 地址 |
| `llm.openrouter.models.lite` | 轻量模型 |

---

## embedding 向量嵌入

| 配置键 | 说明 | 可选值 |
|:---|:---|:---|
| `embedding.provider` | 嵌入服务提供商 | `bytedance` / `siliconflow` |
| `embedding.api-key` | API Key | — |
| `embedding.url` | API 地址 | — |
| `embedding.model` | 嵌入模型名称 | — |

---

## search 网络搜索

| 配置键 | 说明 | 可选值 |
|:---|:---|:---|
| `search.provider` | 搜索提供商 | `exa` / `minimax` |
| `search.api-key` | API Key | — |
| `search.url` | 搜索 API 地址 | — |

---

## vision 视觉分析

| 配置键 | 说明 |
|:---|:---|
| `vision.provider` | 视觉服务提供商 |
| `vision.api-key` | API Key |
| `vision.url` | API 地址 |

---

## browser 浏览器截图

| 配置键 | 说明 |
|:---|:---|
| `browser.download` | 是否自动下载浏览器驱动 |
| `browser.playwright-url` | Playwright 服务地址 |
| `browser.external-host` | 外部访问地址 |

---

## proxy 代理

| 配置键 | 说明 |
|:---|:---|
| `proxy.http` | HTTP 代理地址 |
| `proxy.socks` | SOCKS 代理地址 |

---

## onebot QQ 连接

### 全局设置

| 配置键 | 说明 |
|:---|:---|
| `onebot.ws` | WebSocket 连接地址 |
| `onebot.token` | 连接 Token |

### Bot 设置

| 配置键 | 说明 |
|:---|:---|
| `onebot.bots.<name>.ws` | Bot 的 WebSocket 地址 |
| `onebot.bots.<name>.token` | Bot 的连接 Token |
| `onebot.bots.<name>.role-id` | Bot 人格 ID |
| `onebot.bots.<name>.self-id` | Bot 自身 QQ 号 |
| `onebot.bots.<name>.server-host` | 服务端内网地址 |
| `onebot.bots.<name>.external-host` | 服务端外网地址 |

### 群组设置

| 配置键 | 说明 |
|:---|:---|
| `onebot.bots.<name>.groups.<群号>.admins` | 群管理员 QQ 号列表 |
| `onebot.bots.<name>.groups.<群号>.desire` | 群活跃度，0.0-1.0 |

### 群组覆盖

| 配置键 | 说明 |
|:---|:---|
| `onebot.bots.<name>.groups-override.enable-groups` | 覆盖全局群组白名单 |
| `onebot.bots.<name>.groups-override.debug-group-id` | 覆盖全局调试群 |
| `onebot.bots.<name>.groups-override.message-redirect-map` | 消息转发映射 |

### 插件控制

| 配置键 | 说明 |
|:---|:---|
| `onebot.bots.<name>.enabled-plugins` | 白名单模式，仅启用指定插件 |
| `onebot.bots.<name>.disabled-plugins` | 黑名单模式，禁用指定插件 |

两者同时存在时以白名单为准。`builtin_` 前缀的插件始终启用。

---

## groups 全局群组

| 配置键 | 说明 |
|:---|:---|
| `groups.enable-groups` | 全局启用的群号，逗号分隔 |
| `groups.debug-group-id` | 调试群号 |
| `groups.message-redirect-map` | 消息转发规则，格式 `源群:目标群` |

---

## security 管理认证

| 配置键 | 说明 |
|:---|:---|
| `security.username` | 管理页面用户名 |
| `security.password` | 管理页面密码 |

---

## migration 数据库迁移

| 配置键 | 说明 |
|:---|:---|
| `migration` | 设为 `true` 时启动自动执行数据库迁移 |
