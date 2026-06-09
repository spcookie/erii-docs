# 插件开发概述

erii 的插件系统基于 **PF4J 3.15+** 框架，通过 `uesugi.erii-plugin` Gradle 约定插件统一构建流程。插件以 zip 包形式分发，运行时由 PF4J 的 `DefaultPluginManager` 加载和管理生命周期。

## 扩展接口体系

插件通过实现以下四种扩展接口来接入不同的系统机制：

| 接口 | 完整类路径 | 触发机制 | 适用场景 |
|:---|:---|:---|:---|
| `AgentExtension<T>` | `uesugi.spi.AgentExtension` | 始终参与 Agent 消息循环 | 需要在每次消息处理中参与的通用扩展 |
| `RouteExtension<T>` | `uesugi.spi.RouteExtension` | LLM RoutingAgent 意图分类后按 matcher 匹配调用 | 特定意图触发的功能（如图片搜索、音乐推荐） |
| `CmdExtension<T, Arg, Plugin>` | `uesugi.spi.CmdExtension` | 用户发送 `/xxx` 命令，匹配 `cmd` 和 `alias` | 用户可主动调用的命令式功能 |
| `PassiveExtension<T>` | `uesugi.spi.PassiveExtension` | 不响应消息事件，仅加载时初始化 | 后台任务、HTTP 路由注册、定时作业 |

`T` 泛型参数指定插件类（必须是 `AgentPlugin` 的子类），框架在运行时通过反射解析泛型类型获取对应的 `PluginWrapper`。

### AgentPlugin 基类

所有插件类必须继承抽象类 `AgentPlugin`：

```kotlin
abstract class AgentPlugin : Plugin() {
    fun setWrapper(wrapper: PluginWrapper) {
        PLUGIN_CONTEXT_HOLDER[this::class] = wrapper
    }
}
```

`setWrapper()` 方法由 PF4J 框架在加载插件时调用，将 `PluginWrapper` 实例注册到全局 `PLUGIN_CONTEXT_HOLDER` 映射中。扩展通过 `ref` 属性反查对应的插件类和 wrapper。

## PluginContext 能力矩阵

插件通过 `PluginContext` 接口获得与 erii 核心系统的完整交互能力：

| 属性 | 类型 | 说明 |
|:---|:---|:---|
| `mem` | `Mem` | 内存缓存，支持 AFTER_WRITE / AFTER_ACCESS 两种过期策略 |
| `kv` | `Kv` | 键值持久化存储（基于文件或数据库），同样支持过期策略 |
| `blob` | `Blob` | 二进制文件存储，基于 Okio Path 进行 get/set/delete |
| `vector` | `Vector` | 向量嵌入与语义检索：`embedding()` 生成向量，`search()` 语义搜索，`upsert()` 写入索引 |
| `config` | `PluginConfig` | 插件配置文件读取，支持三层优先级合并和环境变量引用 |
| `database` | `Database` | 数据库访问，提供 `getHistory()` 查询聊天历史 |
| `scheduler` | `Scheduler` | 任务调度器（见下方详细接口） |
| `llm` | `PromptExecutor` | LLM 调用接口，基于 Koog Prompt 引擎 |
| `http` | `HttpClient` | Ktor HttpClient，用于发起 HTTP 请求 |
| `http.proxy` | `HttpClient` | 带代理的 HTTP 客户端（通过 conf/application.conf 中的代理配置） |
| `server` | `Server` | 服务端路由注册：`route {}` 添加自定义 HTTP 端点，`url` 获取当前服务器 URL |

### Scheduler 调度器接口

```kotlin
interface Scheduler {
    // Cron 表达式周期调度
    fun scheduleRecurrently(id: String, cron: String, action: () -> Unit)

    // Kotlin Duration 间隔调度（如 5.minutes）
    fun scheduleRecurrently(id: String, interval: Duration, action: () -> Unit)

    // 一次性延迟任务
    fun schedule(id: String, delay: Duration, action: () -> Unit)

    // 立即入队执行
    fun enqueue(id: String, action: () -> Unit)

    // 取消已注册的任务
    fun cancel(id: String)
}
```

底层基于 JobRunr 实现，`scheduleRecurrently` 将在 JobRunr 面板（`:8000`）中可见。

### Mem / Kv 缓存接口

```kotlin
interface Mem : AutoCloseable {
    suspend fun get(key: String): String?
    suspend fun set(key: String, value: String)
    suspend fun set(key: String, value: String, expire: Duration, strategy: ExpireStrategy)
    suspend fun delete(key: String)
}

enum class ExpireStrategy { AFTER_WRITE, AFTER_ACCESS }
```

`mem` 和 `kv` 接口完全相同，但 `mem` 是进程内存缓存（重启丢失），`kv` 是持久化存储（重启保留）。

### Meta 消息元数据

```kotlin
interface Meta {
    val botId: String        // 当前 Bot ID
    val groupId: String      // 当前群组 ID
    val roledBot: IBotManage.RoledBot  // Bot 角色信息（含 refBot 引用）
    val input: String?       // 用户输入文本（可能为空）
    val senderId: String?    // 发送者 ID
    val echo: String?        // 消息回显 ID
}
```

### Meta 的关键方法

**sendAgent()** — 向 LLM Agent 发送输入并触发工具调用：

```kotlin
// 简单调用
fun Meta.sendAgent(input: String, state: SendAgentStateDsl? = null)

// 带配置的完整调用
fun Meta.sendAgent(
    input: String,
    conf: SendAgentConfig = EmptyConfig,
    state: SendAgentStateDsl? = null
)
```

`SendAgentConfig` 支持的配置项：

| 配置项 | 类型 | 说明 |
|:---|:---|:---|
| `WebSearch.ENABLE/DISABLE` | enum | 是否启用 Web 搜索 |
| `ChatVision.ENABLE/DISABLE` | enum | 是否启用多模态视觉 |
| `ToolSetBuilder { tool { ... } }` | builder | 注入自定义工具集 |
| `Feature(PSFeature)` | object | 主动发言特性标志（如 GRAB, FALLBACK） |

**getRefBot() / getAdmins() / isAdmin()** — 便捷访问函数：

```kotlin
fun Meta.getRefBot() = roledBot.refBot
fun Meta.getAdmins() = ConfigHolder.getAdmins(BotManage.getConfigKey(botId), groupId)
fun Meta.isAdmin() = senderId in getAdmins()
```

## 插件注册方法

在 `onLoad(context: PluginContext)` 中注册扩展功能：

```kotlin
override fun onLoad(context: PluginContext) {
    // 注册消息处理链（handler）
    context.chain { meta ->
        // meta 是当前消息的 Meta 对象
        // 在此处理消息逻辑
    }

    // 注册 LLM 工具集（Agent 可调用的工具）
    context.tool {
        { MyToolSet(context) }  // 返回 MetaToolSetCreator
    }
}
```

- `context.chain(handler)` — 注册一个消息处理器。每次群组收到消息时，所有已注册的 chain handler 按顺序执行。
- `context.tool(toolset)` — 注册一个 LLM 工具集工厂。Agent 可通过 Function Calling 调用工具集中的 `@Tool` 标注方法。

## 两种开发模式

| 模式 | 定义方式 | 代码生成 | 适用场景 |
|:---|:---|:---|:---|
| **类开发模式** | 继承 `AgentPlugin()`，实现 `AgentExtension<T>` 等接口 | 无（手写所有代码） | 复杂插件、需要精细控制生命周期和扩展行为 |
| **注解开发模式** | `@file:Definition` 文件注解 + `@Route`/`@Cmd`/`@Passive`/`@LLMTool` 等功能注解 | KSP 编译时生成 `GeneratedRoute_*`、`GeneratedCmd_*`、`GeneratedPassive_*`、`GeneratedToolSet_*`、`GeneratedPlugin` 及 `META-INF/services/org.pf4j.Extension` | 简单到中等复杂度的插件，声明式定义，减少样板代码 |

两种模式可混合使用（如 `animal` 插件使用类模式，`lolisuki` 使用注解模式）。

## 插件打包

插件通过 `uesugi.erii-plugin` Gradle 约定插件构建：

```kotlin
// erii-plugins/my-plugin/build.gradle.kts
plugins {
    id("uesugi.erii-plugin")
}

version = "0.0.1"
```

构建产物位于 `build/plugin/` 目录，为一个包含以下内容的 zip 文件：

- `classes/` — 编译后的 `.class` 文件
- `lib/` — 依赖 jar 包
- `META-INF/` — `MANIFEST.MF`、`services/org.pf4j.Extension`（扩展注册文件）、`plugin.properties`（插件元数据）

此 zip 文件可直接放入 erii 项目的 `plugins/` 目录加载。
