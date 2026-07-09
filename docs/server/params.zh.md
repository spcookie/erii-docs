# 启动参数

## 修改端口

默认端口为 `8080`。如需更改，使用 `-port` 参数：

```bash
erii server -port 9090
```

## 任务管理面板

erii 内置了一个任务管理面板，可以查看定时任务和后台作业的运行情况。

在 `.env.local` 中开启：

```ini
ERII_JOBRUNR_DASHBOARD=true
ERII_JOBRUNR_DASHBOARD_PORT=8000
```

开启后访问 `http://localhost:8000`。

## 数据库查看面板

erii 使用本地数据库存储数据。开启数据库面板可以直观地查看和操作数据。

在 `.env.local` 中开启：

```ini
ERII_H2_CONSOLE=true
ERII_H2_CONSOLE_PORT=8082
```

开启后访问 `http://localhost:8082`。

- JDBC URL：`jdbc:h2:file:./store/data`
- 用户名：`sa`
- 密码：留空
