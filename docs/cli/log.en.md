# `erii log`

Log viewer TUI panel, similar to `tail` command, with scrollable browsing and keyword highlighting.

```bash
erii log [error] [flags]
```

## Arguments

| Argument | Description               |
|----------|---------------------------|
| (none)   | View `info.log` (default) |
| `error`  | View `error.log`          |

## Options

| Option          | Description                                | Default  |
|-----------------|--------------------------------------------|----------|
| `-n`, `--lines` | Show last N lines                          | `500`    |
| `--logs-path`   | Global option, specify logs directory path | `./logs` |

## Examples

```bash
# View last 500 lines of info.log
erii log

# View last 500 lines of error.log
erii log error

# View last 100 lines
erii log -n 100

# View last 100 lines of error.log
erii log error -n 100

# Custom logs directory
erii log --logs-path /var/log/erii
erii log --logs-path /var/log/erii error -n 200
```

## Keybindings

| Key              | Description    |
|------------------|----------------|
| `↓` / `j`        | Scroll down    |
| `↑` / `k`        | Scroll up      |
| `ctrl+d`         | Half page down |
| `ctrl+u`         | Half page up   |
| `g` / `Home`     | Jump to top    |
| `G` / `End`      | Jump to bottom |
| `esc` / `ctrl+c` | Quit           |

## Log Highlighting

Log lines are parsed and colored using the Logback format:

- Timestamp — cyan
- Level — `ERROR` red/bold, `WARN` yellow/bold, `INFO` green, `DEBUG` blue, `TRACE` gray
- Thread — gray
- Logger — magenta/bold
- Message — white

Lines not matching the Logback format (e.g., stack traces, multi-line messages) are displayed in gray.
