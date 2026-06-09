# Starting the Server

```bash
erii server
```

The server listens on `http://localhost:8080` by default.

## Sub-commands

| Command | Description |
|:---|:---|
| `erii server start` | Start the server (default) |
| `erii server stop` | Stop the server |
| `erii server status` | View server status |
| `erii server restart` | Restart the server |
| `erii server logs` | View server logs |

## Changing Default Credentials

!!! danger "Security Warning"
    Make sure to change the default admin panel username and password after the first start!

The default credentials are `eriix / @Aa123!`. Change them via environment variables:

```bash
# Linux / macOS
export ERII_SERVER_USERNAME="your-username"
export ERII_SERVER_PASSWORD="your-strong-password"

# Windows PowerShell
$env:ERII_SERVER_USERNAME="your-username"
$env:ERII_SERVER_PASSWORD="your-strong-password"
```

You can also configure them in the `.env.local` file:

```ini
ERII_SERVER_USERNAME=your-username
ERII_SERVER_PASSWORD=your-strong-password
```

If the default credentials are not changed, the server will print a yellow warning message in the console on startup.
