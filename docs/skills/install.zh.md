# 安装 erii-plugin-skill

`erii-plugin-skill` 是面向 agent 的开发技能包，用来帮助支持 skill 机制的 agent 创建、修改、审查和脚手架化 Erii 插件项目。

它不是 Erii 运行时插件，不会被放进 Erii 项目的 `plugins/` 目录，也不参与 `erii plugin refresh`。它安装到 agent 的 skill 环境中，供 agent 在开发 Erii 插件时读取公共 SPI、注解模式、`PluginContext`、`erii-common` API 和 Gradle 脚手架约定。

## 适用场景

- 本地有 Erii 源码，希望 agent 按现有 `erii-spi`、`erii-common`、`erii-plugin-gradle` 和示例插件开发。
- 本地没有 Erii 源码，只有依赖包，希望 agent 仍能通过 `llms.txt` 和 `references/` 理解公共 API。
- 需要生成 annotation mode 或 class-based PF4J/SPI mode 的插件骨架。
- 需要让 agent 判断应该使用 `@Cmd`、`@Route`、`@Passive`、`@LLMTool`、`PluginContext` 还是 `ArgParserHolder`。

## 前置要求

- Node.js 18+
- 一个支持 skill 包的 agent runtime
- `npx` 可用

## 安装

`npx skills` 的 `--agent` 参数取决于你使用的 agent runtime。下面使用 `<agent>` 作为占位符：

```bash
npx skills add https://github.com/spcookie/erii-plugin-skill.git --global --agent <agent> --skill erii-plugin-skill --yes
```

例如，如果你的 agent id 是 `codex`：

```bash
npx skills add https://github.com/spcookie/erii-plugin-skill.git --global --agent codex --skill erii-plugin-skill --yes
```

查看已安装的 skill：

```bash
npx skills list --global --agent <agent>
```

## 安装后如何使用

在支持 skill 的 agent 中提出 Erii 插件开发任务时，直接说明目标即可。例如：

```text
创建一个 Erii 插件，提供 /roll 命令，使用 class-based PF4J/SPI 模式。
```

或：

```text
帮我审查这个 Erii annotation plugin 是否正确使用 @ChatMessage 和 useToolMeta()。
```

agent 会读取 `erii-plugin-skill/SKILL.md`，并按需查阅：

- `llms.txt`：无源码环境的快速入口。
- `references/erii-plugin-api.md`：公共 SPI、annotation API、`PluginContext`、调度、存储和工具注册。
- `references/erii-common-api.md`：由插件 Gradle 约定传递提供的 `erii-common` API。
- `references/development-modes.md`：annotation mode 与 class-based PF4J/SPI mode 的选择。
- `references/context-helpers.md`：annotation helper 的生命周期和 coroutine context 边界。
- `references/gradle-projects.md`：Gradle 项目结构、`plugin.json`、`schema.json` 和脚手架脚本。

## 和 Erii 插件安装的区别

| 项目 | erii-plugin-skill | Erii 运行时插件 |
|:---|:---|:---|
| 安装位置 | agent 的 skill 环境 | Erii 项目的 `plugins/` 目录或 npm 依赖 |
| 用途 | 帮助 agent 开发插件 | 扩展正在运行的 Erii bot |
| 安装命令 | `npx skills add ...` | `npm install ...` 或复制插件 zip |
| 是否需要 `erii plugin refresh` | 不需要 | 运行时更新时需要 |

如果你要安装真正运行在 Erii 里的插件，请阅读[安装插件](../plugins/install.md)。

## 更新

重新运行安装命令即可获取最新版本：

```bash
npx skills add https://github.com/spcookie/erii-plugin-skill.git --global --agent <agent> --skill erii-plugin-skill --yes
```
