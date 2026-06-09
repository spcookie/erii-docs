# 启动服务

## 基本用法

```bash
erii server
```

启动后访问 `http://localhost:8080` 可以进入管理页面。

## 常用操作

| 命令 | 作用 |
|:---|:---|
| `erii server start` | 后台启动服务 |
| `erii server stop` | 停止服务 |
| `erii server status` | 查看服务是否在运行 |
| `erii server restart` | 重启服务 |
| `erii server logs` | 查看服务日志 |

`erii server logs` 支持两个参数：

```bash
erii server logs --tail 50   # 查看最近 50 行日志
erii server logs --all       # 查看完整日志
```

## 前台运行

调试时可以让服务在前台运行，日志直接输出到终端：

```bash
erii server --foreground
```

## 修改管理密码

!!! danger "安全提醒"
    首次启动后务必修改默认密码。默认用户名和密码为 `eriix / @Aa123!`。

在你的项目目录下找到 `.env.local` 文件，添加以下内容：

```ini
ERII_SERVER_USERNAME=你的用户名
ERII_SERVER_PASSWORD=你的密码
```

修改后重启服务生效。
