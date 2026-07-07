# Application Configuration

`application.conf` is the main configuration file, located in the project `conf/` directory.

## LLM Configuration

### General Settings

| Key                   | Description         | Allowed Values                                   |
|:----------------------|:--------------------|:-------------------------------------------------|
| `llm.choice-provider` | Default AI provider | `google` / `openai` / `anthropic` / `openrouter` |

### Capability Declaration

Tells the framework which features the selected provider supports for each model tier.
Top-level `llm.capability.*` switches are the default capabilities; `lite`, `flash`, and `pro`
override the corresponding tier.

| Key                               | Description                             |
|:----------------------------------|:----------------------------------------|
| `llm.capability.completion`       | Supports text completion                |
| `llm.capability.prompt-caching`   | Supports prompt caching                 |
| `llm.capability.temperature`      | Supports temperature sampling           |
| `llm.capability.tools`            | Supports function calling / tools       |
| `llm.capability.tool-choice`      | Supports forced tool choice             |
| `llm.capability.multiple-choices` | Supports multiple candidates            |
| `llm.capability.thinking`         | Supports thinking / reasoning mode      |
| `llm.capability.vision-image`     | Supports image input                    |
| `llm.capability.lite.*`           | Capability overrides for the lite tier  |
| `llm.capability.flash.*`          | Capability overrides for the flash tier |
| `llm.capability.pro.*`            | Capability overrides for the pro tier   |

### Parameter Overrides

`llm.param.lite`, `llm.param.flash`, and `llm.param.pro` can be used to override default
LLM parameters (such as `temperature`, `maxTokens`, etc.) for each model tier.

### Providers

```hocon
llm {
  choice-provider: "google"

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

Each provider supports the same set of keys: `api-key`, `base-url`, and `models.{lite,flash,pro}`.

### Usage Pricing

| Key                                       | Description               |
|:------------------------------------------|:--------------------------|
| `llm.usage-pricing.price-unit`            | Pricing unit, e.g. `USD`  |
| `llm.usage-pricing.lite.input-cache-hit`  | Lite cached input price   |
| `llm.usage-pricing.lite.input-cache-miss` | Lite uncached input price |
| `llm.usage-pricing.lite.output`           | Lite output price         |
| `llm.usage-pricing.flash.*`               | Flash tier prices         |
| `llm.usage-pricing.pro.*`                 | Pro tier prices           |

---

## State Tuning

### Emotion

| Key                                              | Description                   |
|:-------------------------------------------------|:------------------------------|
| `state-tuning.emotion.emotion-retention-high`    | High emotion retention rate   |
| `state-tuning.emotion.emotion-retention-medium`  | Medium emotion retention rate |
| `state-tuning.emotion.emotion-retention-low`     | Low emotion retention rate    |
| `state-tuning.emotion.mood-retention`            | Mood retention rate           |
| `state-tuning.emotion.mood-emotion-influence`    | Mood influence on emotion     |
| `state-tuning.emotion.emotion-half-life-seconds` | Emotion half-life in seconds  |
| `state-tuning.emotion.mood-half-life-seconds`    | Mood half-life in seconds     |

### Flow

| Key                                                    | Description                        |
|:-------------------------------------------------------|:-----------------------------------|
| `state-tuning.flow.min-ratio-of-desire`                | Minimum desire ratio               |
| `state-tuning.flow.min-value-min`                      | Minimum value lower bound          |
| `state-tuning.flow.min-value-max`                      | Minimum value upper bound          |
| `state-tuning.flow.decay-normal-per-minute`            | Normal decay per minute            |
| `state-tuning.flow.decay-negative-per-minute`          | Negative decay per minute          |
| `state-tuning.flow.core-interest-base-charge`          | Core interest base charge          |
| `state-tuning.flow.continuous-interaction-base-charge` | Continuous interaction base charge |
| `state-tuning.flow.deep-reply-base-charge`             | Deep reply base charge             |
| `state-tuning.flow.group-resonance-base-charge`        | Group resonance base charge        |
| `state-tuning.flow.negative-penalty`                   | Negative penalty                   |
| `state-tuning.flow.topic-interrupt-penalty`            | Topic interrupt penalty            |
| `state-tuning.flow.repeat-topic-penalty`               | Repeat topic penalty               |
| `state-tuning.flow.low-activity-penalty`               | Low activity penalty               |
| `state-tuning.flow.getting-better-threshold`           | Getting-better threshold           |
| `state-tuning.flow.burst-threshold`                    | Burst threshold                    |

### Volition

| Key                                                      | Description                      |
|:---------------------------------------------------------|:---------------------------------|
| `state-tuning.volition.base-desire-default`              | Default base desire              |
| `state-tuning.volition.keyword-hit-max-stimulus`         | Maximum keyword-hit stimulus     |
| `state-tuning.volition.busy-group-stimulus`              | Busy group stimulus              |
| `state-tuning.volition.indirect-mention-stimulus`        | Indirect mention stimulus        |
| `state-tuning.volition.emotional-resonance-stimulus`     | Emotional resonance stimulus     |
| `state-tuning.volition.reset-stimulus-amount`            | Reset stimulus amount            |
| `state-tuning.volition.fatigue-on-speak`                 | Fatigue on speaking              |
| `state-tuning.volition.fatigue-decay-low-arousal`        | Fatigue decay at low arousal     |
| `state-tuning.volition.fatigue-decay-normal`             | Normal fatigue decay             |
| `state-tuning.volition.stimulus-decay-normal`            | Normal stimulus decay            |
| `state-tuning.volition.stimulus-decay-high-flow`         | Stimulus decay at high flow      |
| `state-tuning.volition.normal-speak-threshold`           | Normal speak threshold           |
| `state-tuning.volition.high-flow-speak-threshold`        | High-flow speak threshold        |
| `state-tuning.volition.high-flow-threshold`              | High-flow threshold              |
| `state-tuning.volition.arousal-impulse-weight`           | Arousal impulse weight           |
| `state-tuning.volition.negative-pleasure-penalty-weight` | Negative pleasure penalty weight |
| `state-tuning.volition.flow-bonus-start`                 | Flow bonus start value           |
| `state-tuning.volition.flow-bonus-weight`                | Flow bonus weight                |

### Memory

| Key                                     | Description                        |
|:----------------------------------------|:-----------------------------------|
| `state-tuning.memory.batch-limit`       | Max messages per batch             |
| `state-tuning.memory.min-messages`      | Minimum messages to trigger memory |
| `state-tuning.memory.stale-recall-days` | Stale recall days                  |

### Summary

| Key                                   | Description                         |
|:--------------------------------------|:------------------------------------|
| `state-tuning.summary.batch-limit`    | Max messages per batch              |
| `state-tuning.summary.min-messages`   | Minimum messages to trigger summary |
| `state-tuning.summary.retention-days` | Summary retention days              |

### Meme

| Key                                         | Description                     |
|:--------------------------------------------|:--------------------------------|
| `state-tuning.meme.analyze-threshold`       | Required seen count to analyze  |
| `state-tuning.meme.max-contexts`            | Max contexts to keep            |
| `state-tuning.meme.cleanup-days`            | Cleanup unused memes after days |
| `state-tuning.meme.low-heat-seen-threshold` | Low-heat seen threshold         |

### Evolution

| Key                                              | Description                          |
|:-------------------------------------------------|:-------------------------------------|
| `state-tuning.evolution.default-weight`          | Default weight                       |
| `state-tuning.evolution.active-weight-threshold` | Active weight threshold              |
| `state-tuning.evolution.min-weight-threshold`    | Minimum weight threshold             |
| `state-tuning.evolution.increase-on-use`         | Weight increase on use               |
| `state-tuning.evolution.decay-per-cycle`         | Decay per cycle                      |
| `state-tuning.evolution.decrease-on-negative`    | Weight decrease on negative feedback |
| `state-tuning.evolution.stale-days`              | Stale days                           |
| `state-tuning.evolution.recent-range-hours`      | Recent range in hours                |
| `state-tuning.evolution.recent-message-limit`    | Recent message limit                 |
| `state-tuning.evolution.active-limit`            | Active word limit                    |

---

## OneBot Connection

Multiple bots can be configured under `onebot.bots`, each as a named block.

```hocon
onebot {
  bots {
    erii {
      ws: "ws://127.0.0.1:3001"
      token: ${?NAPCAT_TOKEN}
      role-id: "erii"
      groups {
        123456789 {
          admins: []
          desire: 15
        }
      }
      enabled-plugins: []
      disabled-plugins: []
    }
  }
}
```

### Bot Settings

| Key                          | Description           |
|:-----------------------------|:----------------------|
| `onebot.bots.<name>.ws`      | Bot WebSocket address |
| `onebot.bots.<name>.token`   | Bot connection token  |
| `onebot.bots.<name>.role-id` | Bot persona ID        |

### Group Settings

| Key                                     | Description                   |
|:----------------------------------------|:------------------------------|
| `onebot.bots.<name>.groups.<id>.admins` | Group admin QQ numbers        |
| `onebot.bots.<name>.groups.<id>.desire` | Group activity / desire level |

### Group Overrides

| Key                                                       | Description                                  |
|:----------------------------------------------------------|:---------------------------------------------|
| `onebot.bots.<name>.groups-override.enable-groups`        | Override global group whitelist              |
| `onebot.bots.<name>.groups-override.debug-group-id`       | Override global debug group                  |
| `onebot.bots.<name>.groups-override.message-redirect-map` | Message redirect map, format `source:target` |

### Plugin Control

| Key                                   | Description                      |
|:--------------------------------------|:---------------------------------|
| `onebot.bots.<name>.enabled-plugins`  | Whitelist only specified plugins |
| `onebot.bots.<name>.disabled-plugins` | Blacklist specified plugins      |

When both are present, the whitelist takes precedence.

---

## Group Configuration

```hocon
groups {
  enable-groups: []
  debug-group-id: null
  message-redirect-map: []
}
```

| Key                           | Description                                    |
|:------------------------------|:-----------------------------------------------|
| `groups.enable-groups`        | Globally enabled group IDs                     |
| `groups.debug-group-id`       | Debug group ID                                 |
| `groups.message-redirect-map` | Message redirect rules, format `source:target` |

---

## Other Components

```hocon
embedding {
  provider: "bytedance"
  api-key: ${?EMBEDDING_API_KEY}
  url: "https://ark.cn-beijing.volces.com/api/v3/embeddings/multimodal"
  model: "doubao-embedding-vision-251215"
}

search {
  provider: "exa"
  api-key: ${?SEARCH_API_KEY}
  url: "https://api.exa.ai/search"
}

vision {
  provider: "minimax"
  api-key: ${?VISION_API_KEY}
  url: "https://api.minimaxi.com/v1/coding_plan/vlm"
}

browser {
  download: false
  playwright-url: ${?PLAYWRIGHT_HOST}
  external-host: ${?EXTERNAL_HOST}
}

proxy {
  http: ${?HTTP_PROXY}
  socks: ${?SOCKS_PROXY}
}
```

---

## Security

> Not included in the default `application.conf`. Add manually if you need admin/UI authentication.

| Key                 | Description    |
|:--------------------|:---------------|
| `security.username` | Admin username |
| `security.password` | Admin password |

---

## Migration

> Not included in the default `application.conf`. Add manually to run database migrations on startup.

| Key         | Description                                             |
|:------------|:--------------------------------------------------------|
| `migration` | Set to `true` to execute database migrations on startup |
