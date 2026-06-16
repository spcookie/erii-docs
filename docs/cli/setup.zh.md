# erii setup

交互式配置向导，一步步引导你完成 erii 的初始设置。

```bash
erii setup
```

## 向导步骤

### 第一步：AI 模型配置

选择 AI 提供商并填写 API Key。支持的提供商：

| 提供商 | 需要准备 |
|:---|:---|
| Google Gemini | Gemini API Key |
| OpenAI | OpenAI API Key |
| Anthropic | Anthropic API Key |
| OpenRouter | OpenRouter API Key |

只需配置一个即可。推荐 Google Gemini，性价比高。

### 第二步：工具配置（可选）

- **向量嵌入**：用于记忆和检索，推荐默认的 ByteDance Embedding
- **网络搜索**：让 erii 能搜索实时信息
- **视觉分析**：让 erii 能理解图片内容
- **浏览器截图**：让 erii 能抓取网页

### 第三步：QQ 连接

- **WebSocket 地址**：NapCat 连接地址，默认 `ws://127.0.0.1:3001`
- **Token**：NapCat 的 Access Token
- **管理员 QQ**：管理员 QQ 号

### 第四步：群组配置

- **启用群组**：让 erii 在哪些群中工作
- **消息转发**：将 A 群的消息转发到 B 群
- **调试群组**：指定一个群用于测试新功能

## 配置完成后

向导会在项目目录生成以下文件：

| 文件 | 内容 |
|:---|:---|
| `conf/application.conf` | 应用配置 |
| `conf/.env.local` | API Key 等敏感信息 |
| `conf/souls/` | Bot 人格描述 |
| `conf/rules/` | 行为规则 |
