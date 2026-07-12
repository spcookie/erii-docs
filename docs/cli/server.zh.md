# erii server

管理 erii 后端服务（基于 Java 的 Ktor 服务）。

```bash
erii server [start]
```

不带子命令时默认执行 `start`。

## 子命令

| 子命令         | 说明                    |
|:------------|:----------------------|
| **start**   | 启动服务（默认），以守护进程方式在后台运行 |
| **stop**    | 停止正在运行的服务             |
| **status**  | 查看服务运行状态              |
| **restart** | 重启服务                  |

## 前台运行

服务默认以守护进程在后台运行。启动时加 `-f` / `--foreground`，可在前台运行，日志直接输出到当前终端，`Ctrl+C` 停止：

```bash
erii server start -f
```

适用于调试，或容器环境（容器通常需要前台常驻进程）。

> `restart` 不支持前台模式，始终以守护进程重启。

## 透传参数给 Java

`start` 和 `restart` 会把多余的参数透传给底层 Java 进程：以 `-D` 开头的作为 JVM 系统属性，其余作为程序参数。

```bash
erii server start -Dsome.prop=value
```

## 常用示例

```bash
erii server            # 启动（后台守护进程）
erii server start -f   # 前台启动
erii server status     # 查看运行状态
erii server restart    # 重启
erii server stop       # 停止
```
