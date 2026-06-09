# Startup Parameters

The erii server supports enabling additional management and debugging panels via JVM system properties.

## Server Port

The default port is `8080`. It can be changed via environment variable or `application.conf`:

```bash
export ERII_PORT=9090
```

Or in `application.conf`:

```hocon
ktor {
  deployment {
    port: 9090
  }
}
```

## JobRunr Dashboard

JobRunr provides a visual management interface for scheduled tasks and background jobs:

```bash
# Enable via environment variable
export ERII_JOBRUNR_DASHBOARD=true

# Or via JVM arguments
java -Djobrunr.dashboard.enabled=true -Djobrunr.dashboard.port=8000 -jar erii.jar
```

| Parameter | Default | Description |
|:---|:---|:---|
| `jobrunr.dashboard.enabled` | `false` | Whether to enable the JobRunr dashboard |
| `jobrunr.dashboard.port` | `8000` | Dashboard port |

Visit `http://localhost:8000` after enabling.

## H2 Database Web Console

Used for directly viewing and manipulating the database:

```bash
# Enable via environment variable
export ERII_H2_CONSOLE=true

# Or via JVM arguments
java -Dh2.console.enabled=true -Dh2.console.port=8082 -jar erii.jar
```

| Parameter | Default | Description |
|:---|:---|:---|
| `h2.console.enabled` | `false` | Whether to enable the H2 console |
| `h2.console.port` | `8082` | Console port |

Visit `http://localhost:8082` after enabling.
