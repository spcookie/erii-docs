# Application Configuration

`application.conf` is the main configuration file, located in the project `conf/` directory.

## LLM Configuration

Each LLM provider requires `api-key`, `base-url`, and model tiers:

```hocon
llm {
  choice-provider: "minimax"

  providers {
    google {
      api-key: ${?GOOGLE_API_KEY}
      base-url: "https://generativelanguage.googleapis.com"
      models {
        lite: "gemini-2.0-flash-lite"
        flash: "gemini-2.0-flash"
        pro: "gemini-2.5-pro"
      }
    }
  }
}
```

Models are organized in three tiers: `lite`, `flash`, `pro`. `choice-provider` sets the default provider.

## OneBot Connection

```hocon
onebot {
  ws: "ws://127.0.0.1:3001"
  token: ${?ONEBOT_TOKEN}
  bots {
    erii {
      ws: "ws://127.0.0.1:3001"
      token: ${?ONEBOT_TOKEN}
      role-id: "default"
      groups {
        "123456789" {
          admins: ["10001"]
          desire: 0.5
        }
      }
      enabled-plugins: "speech, net-ease-music"
    }
  }
}
```

| Key | Description |
|:---|:---|
| `ws` | WebSocket address |
| `token` | Connection token |
| `role-id` | Bot persona ID |
| `groups.<id>.admins` | Group admin QQ numbers |
| `enabled-plugins` | Whitelist specific plugins |
| `disabled-plugins` | Blacklist specific plugins |

## Group Configuration

```hocon
groups {
  enable-groups: "123456789, 987654321"
  debug-group-id: "123456789"
  message-redirect-map: "111:222, 333:444"
}
```

## Other Components

```hocon
embedding {
  provider: "bytedance"
  api-key: ${?EMBEDDING_API_KEY}
}

search {
  provider: "exa"
  api-key: ${?SEARCH_API_KEY}
}

vision {
  provider: "minimax"
  api-key: ${?VISION_API_KEY}
}

browser {
  playwright-url: "ws://127.0.0.1:3000"
}

proxy {
  http: ${?HTTP_PROXY}
  socks: ${?SOCKS_PROXY}
}
```
