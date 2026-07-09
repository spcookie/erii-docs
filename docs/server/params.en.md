# Startup Parameters

## Server Port

The default port is `8080`. Change it with the `-port` flag:

```bash
erii server -port 9090
```

## JobRunr Dashboard

JobRunr provides a visual management interface for scheduled tasks and background jobs.

Enable via `.env.local`:

```ini
ERII_JOBRUNR_DASHBOARD=true
ERII_JOBRUNR_DASHBOARD_PORT=8000
```

| Parameter                     | Default | Description                  |
|:------------------------------|:--------|:-----------------------------|
| `ERII_JOBRUNR_DASHBOARD`      | `false` | Enable the JobRunr dashboard |
| `ERII_JOBRUNR_DASHBOARD_PORT` | `8000`  | Dashboard port               |

Visit `http://localhost:8000` after enabling.

## H2 Database Web Console

Used for directly viewing and manipulating the database.

Enable via `.env.local`:

```ini
ERII_H2_CONSOLE=true
ERII_H2_CONSOLE_PORT=8082
```

| Parameter              | Default | Description           |
|:-----------------------|:--------|:----------------------|
| `ERII_H2_CONSOLE`      | `false` | Enable the H2 console |
| `ERII_H2_CONSOLE_PORT` | `8082`  | Console port          |

Visit `http://localhost:8082` after enabling.
