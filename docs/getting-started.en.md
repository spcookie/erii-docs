# Quick Install

## Prerequisites

- **Node.js** 18+ — see [Appendix: Environment Setup](appendix.md#nodejs)
- **OneBot V11 Server** — Any QQ access service supporting the OneBot V11 protocol (e.g. NapCat, LLOneBot),
  see [Appendix: Environment Setup](appendix.md#napcat)
- **LLM API Key** — at least one LLM provider API Key

## Script Install (Recommended)

The script automatically checks and installs the Node.js environment.

=== "Linux / macOS"

    ```bash
    curl -fsSL https://raw.githubusercontent.com/spcookie/erii-distribution/refs/heads/master/scripts/install.sh | bash
    ```

=== "Windows PowerShell"

    ```powershell
    irm https://raw.githubusercontent.com/spcookie/erii-distribution/refs/heads/master/scripts/install.ps1 | iex
    ```

## Global Install

If you already have Node.js installed:

```bash
npm install -g erii-bot
erii setup
```

## Project Create

Create an erii sub-project within an existing project, suitable for advanced users needing custom project structures. Supports multiple package managers:

=== "npm"

    ```bash
    npm create erii-bot
    ```

=== "pnpm"

    ```bash
    pnpm create erii-bot
    ```

=== "yarn"

    ```bash
    yarn create erii-bot
    ```

=== "bun"

    ```bash
    bun create erii-bot
    ```

After installation, enter the project directory:

```bash
cd erii
```

## Initial Setup

After installation, run the interactive configuration wizard:

```bash
erii setup
```

The wizard guides you through AI model selection, optional tools, QQ connection, and group configuration.

Upon completion, `conf/application.conf` and `conf/.env.local` are generated.

## Next Steps

[Start the server](server/start.md)
