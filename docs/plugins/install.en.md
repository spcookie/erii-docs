# Installing Plugins

## Extension Types

erii is built on the PF4J plugin framework and supports four extension types:

| Extension Type | Description | Trigger Method |
|:---|:---|:---|
| **AgentExtension** | General-purpose Agent extension | Always participates in Agent loop |
| **RouteExtension** | LLM route extension | RoutingAgent intent classification match |
| **CmdExtension** | Command extension | `/xxx` command prefix match, supports aliases |
| **PassiveExtension** | Passive extension | Background tasks / event listeners |

## Built-in Plugins

| Plugin           | Type             | Description                                                           |
|:-----------------|:-----------------|:----------------------------------------------------------------------|
| `speech`         | AgentExtension   | TTS plugin to convert text to voice                                   |
| `lolisuki`       | RouteExtension   | Anime image plugin, fetches and sends images from lolisuki.cn         |
| `net-ease-music` | PassiveExtension | NetEase Music plugin, searches and sends music cards                  |
| `qq-face`        | PassiveExtension | QQ emoji plugin, semantically matches and sends appropriate emojis    |
| `seeddream`      | RouteExtension   | AI image generation plugin, supports text-to-image and image-to-image |
| `rollpig`        | AgentExtension   | Roll Pig game plugin, a collection and nurturing interactive game     |
| `animal`         | AgentExtension   | Virtual pet nurturing plugin, role-playing with affinity system       |

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
