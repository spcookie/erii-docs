# `erii log`

日志查看 TUI 面板，类似 `tail` 命令，支持滚动浏览和关键词高亮。

```bash
erii log [error] [flags]
```

## 参数

| 参数      | 说明                |
|---------|-------------------|
| 无       | 查看 `info.log`（默认） |
| `error` | 查看 `error.log`    |

## 选项

| 选项              | 说明            | 默认值      |
|-----------------|---------------|----------|
| `-n`, `--lines` | 显示最后 N 行      | `500`    |
| `--logs-path`   | 全局选项，指定日志目录路径 | `./logs` |

## 示例

```bash
# 查看 info.log 最后 500 行
erii log

# 查看 error.log 最后 500 行
erii log error

# 查看最后 100 行
erii log -n 100

# 查看 error.log 最后 100 行
erii log error -n 100

# 自定义日志目录
erii log --logs-path /var/log/erii
erii log --logs-path /var/log/erii error -n 200
```

## 快捷键

| 快捷键              | 说明    |
|------------------|-------|
| `↓` / `j`        | 向下滚动  |
| `↑` / `k`        | 向上滚动  |
| `ctrl+d`         | 向下翻半屏 |
| `ctrl+u`         | 向上翻半屏 |
| `g` / `Home`     | 跳到顶部  |
| `G` / `End`      | 跳到底部  |
| `esc` / `ctrl+c` | 退出    |

## 日志高亮

日志行按照 Logback 格式解析着色：

- 时间戳 — 青色
- 级别 — `ERROR` 红/粗, `WARN` 黄/粗, `INFO` 绿, `DEBUG` 蓝, `TRACE` 灰
- 线程 — 灰色
- Logger — 粉紫/粗
- 消息 — 白色

不匹配 Logback 格式的行（如堆栈跟踪、多行消息）以灰色显示。
