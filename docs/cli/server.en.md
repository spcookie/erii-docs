# erii server

Manage the erii backend service (a Java-based Ktor server).

```bash
erii server [start]
```

Without a subcommand, `start` is used by default.

## Subcommands

| Subcommand  | Description                                                 |
|:------------|:------------------------------------------------------------|
| **start**   | Start the service (default), running as a background daemon |
| **stop**    | Stop the running service                                    |
| **status**  | Show the service status                                     |
| **restart** | Restart the service                                         |

## Foreground mode

The service runs as a background daemon by default. Add `-f` / `--foreground`
on start to run in the foreground, streaming logs to the current terminal;
stop with `Ctrl+C`:

```bash
erii server start -f
```

Useful for debugging or container environments (containers usually need a
long-running foreground process).

> `restart` does not support foreground mode; it always restarts as a daemon.

## Passing arguments to Java

`start` and `restart` forward extra arguments to the underlying Java process:
arguments starting with `-D` become JVM system properties, the rest become
program arguments.

```bash
erii server start -Dsome.prop=value
```

## Common examples

```bash
erii server            # start (background daemon)
erii server start -f   # start in the foreground
erii server status     # show status
erii server restart    # restart
erii server stop       # stop
```
