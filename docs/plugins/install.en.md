# Installing Plugins

## Extension Types

erii is built on the PF4J plugin framework and supports four extension types:

| Extension Type | Description | Trigger Method |
|:---|:---|:---|
| **AgentExtension** | General-purpose Agent extension | Always participates in Agent loop |
| **RouteExtension** | LLM route extension | RoutingAgent intent classification match |
| **CmdExtension** | Command extension | `/xxx` command prefix match, supports aliases |
| **PassiveExtension** | Passive extension | Background tasks / event listeners |

## Installation Methods

=== "npm Install"

    ```bash
    # Globally install plugin package
    npm install -g @some-author/erii-plugin-xxx

    # Or install in project
    npm install @some-author/erii-plugin-xxx
    ```

=== "Manual Install"

    Place the plugin zip file into the `plugins/` directory of your erii project:

    ```bash
    cp my-plugin-0.0.1.zip /path/to/erii/plugins/
    ```

After installation, run `erii reload` to reload plugins.
