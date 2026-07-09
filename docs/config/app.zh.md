# 应用配置

应用配置通过 `erii config` 命令修改，无需手动编辑配置文件。

```bash
erii config                    # 打开 TUI 交互式编辑器
erii config app get <键>       # 读取配置项
erii config app set <键> <值>  # 修改配置项
```

---

## LLM 对话模型

### 通用设置

| 配置键                   | 说明        | 可选值                    |
|:----------------------|:----------|:-----------------------|
| `llm.choice-provider` | 默认 AI 提供商 | `openai` / `anthropic` |

### 能力声明

用于告知框架当前所选提供商在不同模型层级上支持哪些特性。`llm.capability` 下的顶层开关为默认能力，
`lite` / `flash` / `pro` 可分别覆盖对应层级的能力。

| 配置键                               | 说明                  |
|:----------------------------------|:--------------------|
| `llm.capability.completion`       | 是否支持文本补全            |
| `llm.capability.prompt-caching`   | 是否支持提示缓存            |
| `llm.capability.temperature`      | 是否支持 temperature 采样 |
| `llm.capability.tools`            | 是否支持函数调用/工具         |
| `llm.capability.tool-choice`      | 是否支持强制指定工具          |
| `llm.capability.multiple-choices` | 是否支持多候选返回           |
| `llm.capability.thinking`         | 是否支持思考/推理模式         |
| `llm.capability.vision-image`     | 是否支持图片输入            |
| `llm.capability.lite.*`           | lite 模型层能力覆盖        |
| `llm.capability.flash.*`          | flash 模型层能力覆盖       |
| `llm.capability.pro.*`            | pro 模型层能力覆盖         |

### 参数覆盖

`llm.param.lite`、`llm.param.flash`、`llm.param.pro` 可用于为各模型层覆盖默认 LLM 参数（如 `temperature`、`maxTokens` 等）。

### OpenAI

| 配置键                                 | 说明      |
|:------------------------------------|:--------|
| `llm.providers.openai.api-key`      | API Key |
| `llm.providers.openai.base-url`     | API 地址  |
| `llm.providers.openai.models.lite`  | 轻量模型    |
| `llm.providers.openai.models.flash` | 快速模型    |
| `llm.providers.openai.models.pro`   | 高质量模型   |

### Anthropic

| 配置键                                    | 说明      |
|:---------------------------------------|:--------|
| `llm.providers.anthropic.api-key`      | API Key |
| `llm.providers.anthropic.base-url`     | API 地址  |
| `llm.providers.anthropic.models.lite`  | 轻量模型    |
| `llm.providers.anthropic.models.flash` | 快速模型    |
| `llm.providers.anthropic.models.pro`   | 高质量模型   |

### 用量计费

| 配置键                                       | 说明             |
|:------------------------------------------|:---------------|
| `llm.usage-pricing.price-unit`            | 价格单位，如 `USD`   |
| `llm.usage-pricing.lite.input-cache-hit`  | lite 缓存命中输入单价  |
| `llm.usage-pricing.lite.input-cache-miss` | lite 缓存未命中输入单价 |
| `llm.usage-pricing.lite.output`           | lite 输出单价      |
| `llm.usage-pricing.flash.*`               | flash 各阶段单价    |
| `llm.usage-pricing.pro.*`                 | pro 各阶段单价      |

---

## state-tuning 状态调参

### emotion 情绪模型

| 配置键                                              | 说明        |
|:-------------------------------------------------|:----------|
| `state-tuning.emotion.emotion-retention-high`    | 高保留率      |
| `state-tuning.emotion.emotion-retention-medium`  | 中保留率      |
| `state-tuning.emotion.emotion-retention-low`     | 低保留率      |
| `state-tuning.emotion.mood-retention`            | 心情保留率     |
| `state-tuning.emotion.mood-emotion-influence`    | 心情对情绪影响权重 |
| `state-tuning.emotion.emotion-half-life-seconds` | 情绪半衰期（秒）  |
| `state-tuning.emotion.mood-half-life-seconds`    | 心情半衰期（秒）  |

### flow 话题参与度

| 配置键                                                    | 说明        |
|:-------------------------------------------------------|:----------|
| `state-tuning.flow.min-ratio-of-desire`                | 最小欲望比例    |
| `state-tuning.flow.min-value-min`                      | 最小价值下限    |
| `state-tuning.flow.min-value-max`                      | 最小价值上限    |
| `state-tuning.flow.decay-normal-per-minute`            | 正常衰减（每分钟） |
| `state-tuning.flow.decay-negative-per-minute`          | 负面衰减（每分钟） |
| `state-tuning.flow.core-interest-base-charge`          | 核心兴趣基础充能  |
| `state-tuning.flow.continuous-interaction-base-charge` | 连续互动基础充能  |
| `state-tuning.flow.deep-reply-base-charge`             | 深度回复基础充能  |
| `state-tuning.flow.group-resonance-base-charge`        | 群体共鸣基础充能  |
| `state-tuning.flow.negative-penalty`                   | 负面惩罚      |
| `state-tuning.flow.topic-interrupt-penalty`            | 话题中断惩罚    |
| `state-tuning.flow.repeat-topic-penalty`               | 重复话题惩罚    |
| `state-tuning.flow.low-activity-penalty`               | 低活跃惩罚     |
| `state-tuning.flow.getting-better-threshold`           | 好转阈值      |
| `state-tuning.flow.burst-threshold`                    | 爆发阈值      |

### volition 发言意愿

| 配置键                                                      | 说明          |
|:---------------------------------------------------------|:------------|
| `state-tuning.volition.base-desire-default`              | 默认基础欲望      |
| `state-tuning.volition.keyword-hit-max-stimulus`         | 关键词命中最大刺激   |
| `state-tuning.volition.busy-group-stimulus`              | 活跃群刺激       |
| `state-tuning.volition.indirect-mention-stimulus`        | 间接提及刺激      |
| `state-tuning.volition.emotional-resonance-stimulus`     | 情绪共鸣刺激      |
| `state-tuning.volition.reset-stimulus-amount`            | 重置刺激量       |
| `state-tuning.volition.fatigue-on-speak`                 | 发言疲劳值       |
| `state-tuning.volition.fatigue-decay-low-arousal`        | 低唤醒疲劳衰减     |
| `state-tuning.volition.fatigue-decay-normal`             | 正常疲劳衰减      |
| `state-tuning.volition.stimulus-decay-normal`            | 正常刺激衰减      |
| `state-tuning.volition.stimulus-decay-high-flow`         | 高 flow 刺激衰减 |
| `state-tuning.volition.normal-speak-threshold`           | 正常发言阈值      |
| `state-tuning.volition.high-flow-speak-threshold`        | 高 flow 发言阈值 |
| `state-tuning.volition.high-flow-threshold`              | 高 flow 阈值   |
| `state-tuning.volition.arousal-impulse-weight`           | 唤醒冲动权重      |
| `state-tuning.volition.negative-pleasure-penalty-weight` | 负面愉悦惩罚权重    |
| `state-tuning.volition.flow-bonus-start`                 | flow 加成起始值  |
| `state-tuning.volition.flow-bonus-weight`                | flow 加成权重   |

### memory 记忆

| 配置键                                     | 说明          |
|:----------------------------------------|:------------|
| `state-tuning.memory.batch-limit`       | 每批处理消息上限    |
| `state-tuning.memory.min-messages`      | 触发记忆所需最小消息数 |
| `state-tuning.memory.stale-recall-days` | 记忆失效天数      |

### summary 摘要

| 配置键                                   | 说明          |
|:--------------------------------------|:------------|
| `state-tuning.summary.batch-limit`    | 每批处理消息上限    |
| `state-tuning.summary.min-messages`   | 触发摘要所需最小消息数 |
| `state-tuning.summary.retention-days` | 摘要保留天数      |

### meme 表情包

| 配置键                                         | 说明         |
|:--------------------------------------------|:-----------|
| `state-tuning.meme.analyze-threshold`       | 触发分析所需出现次数 |
| `state-tuning.meme.max-contexts`            | 保留上下文数量上限  |
| `state-tuning.meme.cleanup-days`            | 清理未使用表情包天数 |
| `state-tuning.meme.low-heat-seen-threshold` | 低热度判定阈值    |

### evolution 词汇进化

| 配置键                                              | 说明       |
|:-------------------------------------------------|:---------|
| `state-tuning.evolution.default-weight`          | 默认权重     |
| `state-tuning.evolution.active-weight-threshold` | 活跃权重阈值   |
| `state-tuning.evolution.min-weight-threshold`    | 最小权重阈值   |
| `state-tuning.evolution.increase-on-use`         | 使用时增量    |
| `state-tuning.evolution.decay-per-cycle`         | 每周期衰减    |
| `state-tuning.evolution.decrease-on-negative`    | 负面反馈减量   |
| `state-tuning.evolution.stale-days`              | 失效天数     |
| `state-tuning.evolution.recent-range-hours`      | 近期范围（小时） |
| `state-tuning.evolution.recent-message-limit`    | 近期消息数量上限 |
| `state-tuning.evolution.active-limit`            | 活跃词上限    |

---

## embedding 向量嵌入

| 配置键 | 说明 | 可选值 |
|:---|:---|:---|
| `embedding.provider` | 嵌入服务提供商 | `bytedance` / `siliconflow` |
| `embedding.api-key` | API Key | — |
| `embedding.url` | API 地址 | — |
| `embedding.model` | 嵌入模型名称 | — |

---

## search 网络搜索

| 配置键 | 说明 | 可选值 |
|:---|:---|:---|
| `search.provider` | 搜索提供商 | `exa` / `minimax` |
| `search.api-key` | API Key | — |
| `search.url` | 搜索 API 地址 | — |

---

## vision 视觉分析

| 配置键 | 说明 |
|:---|:---|
| `vision.provider` | 视觉服务提供商 |
| `vision.api-key` | API Key |
| `vision.url` | API 地址 |

---

## browser 浏览器截图

| 配置键 | 说明 |
|:---|:---|
| `browser.download` | 是否自动下载浏览器驱动 |
| `browser.playwright-url` | Playwright 服务地址 |
| `browser.external-host` | 外部访问地址 |

---

## proxy 代理

| 配置键 | 说明 |
|:---|:---|
| `proxy.http` | HTTP 代理地址 |
| `proxy.socks` | SOCKS 代理地址 |

---

## onebot QQ 连接

`onebot.bots` 下可配置多个机器人，每个机器人为一个命名块。

### Bot 设置

| 配置键 | 说明 |
|:---|:---|
| `onebot.bots.<name>.ws` | Bot 的 WebSocket 地址 |
| `onebot.bots.<name>.token` | Bot 的连接 Token |
| `onebot.bots.<name>.role-id` | Bot 人格 ID |

### 群组设置

| 配置键                                     | 说明          |
|:----------------------------------------|:------------|
| `onebot.bots.<name>.groups.<群号>.admins` | 群管理员 QQ 号列表 |
| `onebot.bots.<name>.groups.<群号>.desire` | 群活跃度/发言欲望   |

### 群组覆盖

| 配置键                                                       | 说明                 |
|:----------------------------------------------------------|:-------------------|
| `onebot.bots.<name>.groups-override.enable-groups`        | 覆盖全局群组白名单          |
| `onebot.bots.<name>.groups-override.debug-group-id`       | 覆盖全局调试群            |
| `onebot.bots.<name>.groups-override.message-redirect-map` | 消息转发映射，格式 `源群:目标群` |

### 插件控制

| 配置键 | 说明 |
|:---|:---|
| `onebot.bots.<name>.enabled-plugins` | 白名单模式，仅启用指定插件 |
| `onebot.bots.<name>.disabled-plugins` | 黑名单模式，禁用指定插件 |

两者同时存在时以白名单为准。

---

## groups 全局群组

| 配置键                           | 说明                 |
|:------------------------------|:-------------------|
| `groups.enable-groups`        | 全局启用的群号列表          |
| `groups.debug-group-id`       | 调试群号（`null` 则不启用）  |
| `groups.message-redirect-map` | 消息转发规则，格式 `源群:目标群` |

---

## security 管理认证

通过环境变量配置管理页面的用户名和密码，参见 [环境变量](env.md#_2)。

| 配置键                 | 说明      |
|:--------------------|:--------|
| `security.username` | 管理页面用户名 |
| `security.password` | 管理页面密码  |
