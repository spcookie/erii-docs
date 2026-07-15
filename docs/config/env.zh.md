# 环境变量

环境变量通过 `erii config` 命令修改，无需手动编辑 `.env.local` 文件。

```bash
erii config                    # 打开 TUI，选择 env 面板
erii config env get <变量名>   # 读取
erii config env set <变量名> <值>  # 修改
```

---

## LLM API Key

各 AI 提供商的 API Key：

| 变量名                 | 对应提供商        |
|:--------------------|:-------------|
| `OPENAI_API_KEY`    | OpenAI 兼容    |
| `ANTHROPIC_API_KEY` | Anthropic 兼容 |

---

## 组件服务

| 变量名 | 说明 |
|:---|:---|
| `EMBEDDING_API_KEY` | 向量嵌入服务 API Key |
| `SEARCH_API_KEY` | 网络搜索 API Key |
| `VISION_API_KEY` | 视觉分析 API Key |

---

## 连接配置

| 变量名 | 说明 |
|:---|:---|
| `ONEBOT_TOKEN` | NapCat 连接 Token |

---

## 服务端配置

| 变量名                           | 默认值       | 说明                |
|:------------------------------|:----------|:------------------|
| `ERII_SERVER_USERNAME`        | `eriix`   | 管理页面用户名           |
| `ERII_SERVER_PASSWORD`        | `@Aa123!` | 管理页面密码            |
| `ERII_PORT`                   | `8080`    | 服务端口              |
| `ERII_START_MODE`             | —         | 设为 `CLI` 时日志写入文件  |
| `ERII_JOBRUNR_DASHBOARD`      | `false`   | 启用任务管理面板          |
| `ERII_JOBRUNR_DASHBOARD_PORT` | `8000`    | 任务面板端口            |
| `ERII_H2_CONSOLE`             | `false`   | 启用数据库面板           |
| `ERII_H2_CONSOLE_PORT`        | `8082`    | 数据库面板端口           |
| `ERII_STORE_DIR`              | `./store` | 数据、对象、向量、图和缓存存储目录 |

---

## 代理配置

| 变量名 | 说明 |
|:---|:---|
| `HTTP_PROXY` | HTTP 代理地址 |
| `SOCKS_PROXY` | SOCKS 代理地址 |
