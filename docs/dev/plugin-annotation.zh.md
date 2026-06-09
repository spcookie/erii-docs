# 注解开发模式

注解开发模式通过声明式注解定义插件功能，由 KSP (Kotlin Symbol Processing) 在编译时自动生成扩展类、工具集和 PF4J 服务注册文件。适合中等复杂度、逻辑相对直接的插件。

以下以 erii 内置的 `lolisuki`（二次元图片插件）为真实示例进行说明。

## 依赖配置

```kotlin
// erii-plugins/my-plugin/build.gradle.kts
plugins {
    id("uesugi.erii-plugin")
}

dependencies {
    // KSP 注解处理器（编译时代码生成）
    add("ksp", project(":erii-spi:erii-spi-annotation"))
    // 注解定义（仅编译时需要，运行时由核心框架提供）
    compileOnly(project(":erii-spi:erii-spi-annotation"))
}

version = "0.0.1"
```

`uesugi.erii-plugin` 约定插件已预配置 KSP 插件，开发者只需声明依赖即可。

## 文件级注解：@file:Definition

每个使用注解模式的插件必须在任意一个 `.kt` 文件的顶部声明文件级 `@Definition` 注解：

```kotlin
// Lolisuki.kt 文件顶部
@file:Definition(
    pluginId = "lolisuki",
    version = "0.0.1",
    description = "二次元涩图插件"
)

package uesugi.plugin
```

| 参数 | 说明 | 必需 |
|:---|:---|:---|
| `pluginId` | 插件唯一标识符 | 是 |
| `version` | 语义化版本号 | 是 |
| `requires` | 依赖的其他插件（PF4J requires 表达式） | 否 |
| `dependencies` | 外部依赖声明 | 否 |
| `description` | 插件描述 | 否 |
| `provider` | 提供者信息 | 否 |
| `license` | 许可证类型 | 否 |

KSP 处理器扫描所有文件找到 `@file:Definition`，自动生成 `GeneratedPlugin` 类（继承 `AgentPlugin`）并注册 PF4J plugin 描述。

## 功能注解详解

### @Route — 路由扩展

将函数注册为 `RouteExtension`，由 LLM RoutingAgent 根据意图分类匹配调用：

```kotlin
@Route(
    path = "REQUEST_R18_IMAGE",
    method = """
        仅当用户【明确提出图片请求】时，才选择此分类。
        必须【同时满足】以下两个条件，缺一不可：
        
        条件一：图片内容明确指向【成人 / R18】
        条件二：用户有索要图片的意图或行为
        
        强制排除规则：
        如果内容仅包含文字但没有任何索要图片的意图
        则绝对不要归为此类。
    """,
    toolSets = ["lolisuki"]
)
suspend fun lolisukiRoute(meta: Meta) {
    // 处理逻辑
    val image = getImage(meta)
    // ...
}
```

| 参数 | 类型 | 说明 |
|:---|:---|:---|
| `method` | String | **意图匹配规则**（LLM 提示词）。不同于 HTTP Method，此处是 LLM RoutingAgent 用来判断是否匹配此路由的分类描述文本 |
| `path` | String | **路由路径标识**。与 `method` 共同构成 `matcher: Pair<String, String>`，path 作为 key，method 作为 value |
| `toolSets` | Array\<String\> | 关联的工具集名称列表，默认 `["default"]` |
| `onLoad` | Array\<String\> | 加载时调用的 `@OnLoad("name")` 函数名列表 |
| `onUnload` | Array\<String\> | 卸载时调用的 `@OnUnload("name")` 函数名列表 |

函数参数可为空或接收 `meta: Meta`。若有 `meta` 参数，KSP 自动注入当前消息元数据。

KSP 生成的扩展类为 `GeneratedRoute_<函数名>`，实现 `RouteExtension<GeneratedPlugin>`：

```kotlin
@Extension
class GeneratedRoute_lolisukiRoute : RouteExtension<GeneratedPlugin> {
    override val matcher: Pair<String, String>
        get() = "REQUEST_R18_IMAGE" to "<LLM规则文本>"

    override fun onLoad(context: PluginContext) {
        context.chain { meta ->
            withPluginContext(context) {
                withMeta(meta) {
                    lolisukiRoute(meta)
                }
            }
        }
        context.tool { { GeneratedToolSet_lolisuki(context) } }
    }
}
```

### @Cmd — 命令扩展

将函数注册为 `SlashCmdExtension`，响应用户的 `/xxx` 命令：

```kotlin
@Cmd(
    name = "hello",
    alias = ["hi", "hey"],
    toolSets = ["default"]
)
suspend fun helloCommand(meta: Meta, args: List<String>) {
    // meta.input 包含完整用户输入
    // args 包含命令参数列表
}
```

| 参数 | 类型 | 说明 |
|:---|:---|:---|
| `name` | String | 命令名（不含斜杠），如 `hello` 对应 `/hello` |
| `alias` | Array\<String\> | 命令别名列表，如 `/hi`、`/hey` |
| `toolSets` | Array\<String\> | 关联的工具集名称列表 |
| `onLoad` | Array\<String\> | 加载时调用的函数名列表 |
| `onUnload` | Array\<String\> | 卸载时调用的函数名列表 |

函数参数接受两个可选位置参数（前缀匹配）：

1. `meta: Meta` — 消息元数据
2. `args: List<String>` — 解析后的命令参数列表

KSP 生成的扩展类为 `GeneratedCmd_<命令名>`（命令名中的 `-` 替换为 `_`），实现 `SlashCmdExtension<GeneratedPlugin>`。

### @Passive — 被动扩展

将函数注册为 `PassiveExtension`，在插件加载时执行：

```kotlin
@Passive(toolSets = ["default"])
suspend fun backgroundTask(meta: Meta) {
    val scheduler = useScheduler()
    scheduler.scheduleRecurrently("task-id", "0 */1 * * *") {
        // 每小时执行的后台任务
    }
}
```

KSP 生成的扩展类为 `GeneratedPassive_<函数名>`。

### @LLMTool — LLM 工具

将函数注册为 Agent 可调用的 Function Calling 工具：

```kotlin
@LLMTool(name = "send_sex_image", set = "lolisuki")
@LLMDesc("发送一张涩图给群友")
suspend fun sendSexImage(): String {
    val meta = useToolMeta().value
    val resource = getImage(meta)
    if (resource != null) {
        val base64 = Base64.getEncoder().encodeToString(resource)
        MetaToolSet.meta.roledBot.refBot.sendGroupMsg(
            meta.groupId.toLong(),
            buildMessage { image("base64://$base64") }
        )
    }
    return "发送成功"
}
```

| 参数 | 类型 | 说明 |
|:---|:---|:---|
| `name` | String | 工具名称（Agent 通过此名称调用），默认为函数名 |
| `set` | String | 所属工具集名称，默认为 `"default"`。同一 `set` 的工具被归类到同一个 `GeneratedToolSet_<set>` 类中 |

函数参数可使用 `@LLMDesc` 注解描述：

```kotlin
@LLMTool(name = "search_images")
@LLMDesc("根据查询字符串搜索图片")
suspend fun searchImages(
    @LLMDesc("搜索关键词") query: String,
    @LLMDesc("返回结果数量") limit: Int = 10
): String {
    // 实现搜索逻辑
}
```

KSP 为每个工具集名称生成一个 `GeneratedToolSet_<set>` 类：

```kotlin
class GeneratedToolSet_lolisuki(private val context: PluginContext) : MetaToolSet {
    @LLMDescription("发送一张涩图给群友")
    @Tool
    suspend fun sendSexImage(): String? = withPluginContext(context) {
        _erii_sendSexImage()
    }
}
```

??? warning "suspend 函数限制"
    `@LLMTool` 标注的函数必须是 `suspend` 函数。非 suspend 函数会被 KSP 自动包装到 `withContext(Dispatchers.IO)` 块中执行。

### @LLMDesc — 工具描述

可标注在函数和参数上：

```kotlin
@LLMDesc("根据关键词发送动漫图片")
@LLMTool(name = "send_anime_image")
suspend fun sendAnimeImage(
    @LLMDesc("动漫角色名或风格关键词") keyword: String,
    @LLMDesc("返回数量(1-10)") count: Int = 3
): String { ... }
```

### 生命周期注解

| 注解 | 作用 | 限定 |
|:---|:---|:---|
| `@OnLoad("name")` | 插件或扩展加载时调用。有 name 参数时可被 `@Route`/`@Cmd`/`@Passive` 的 onLoad 引用 | 函数必须零参数，可 suspend |
| `@OnUnload("name")` | 插件或扩展卸载时调用。与 `@OnLoad` 配对使用 | 函数必须零参数，可 suspend |
| `@OnStart` | Plugin.start() 时调用 | 每个插件最多一个；必须零参数，可 suspend |
| `@OnStop` | Plugin.stop() 时调用 | 每个插件最多一个；必须零参数，可 suspend |

`@OnLoad()` 无 name 参数时为全局 onLoad，所有扩展都会调用；
`@OnLoad("name")` 为命名 onLoad，仅被引用它的扩展调用。

```kotlin
@OnLoad("init_config")
suspend fun initConfig() {
    val config = useConfig()
    // 加载配置
}

@OnUnload("cleanup_config")
suspend fun cleanupConfig() {
    // 清理配置
}

// 此 Route 扩展加载时会自动调用 initConfig()，卸载时调用 cleanupConfig()
@Route(
    path = "MY_FEATURE",
    method = "...",
    onLoad = ["init_config"],
    onUnload = ["cleanup_config"]
)
suspend fun myFeature(meta: Meta) { ... }
```

## 上下文访问函数

在注解标注的函数中，使用以下 suspend 函数访问 PluginContext 的各项能力：

| 函数 | 返回类型 | 对应 PluginContext 属性 |
|:---|:---|:---|
| `useMeta()` | `Meta` | 当前消息元数据（仅在 chain 上下文中可用） |
| `useToolMeta()` | `Lazy<Meta>` | 在 `@LLMTool` 函数中懒获取 Meta |
| `useMem()` | `Mem` | 内存缓存 |
| `useKv()` | `Kv` | 键值存储 |
| `useBlob()` | `Blob` | 文件存储 |
| `useVector()` | `Vector` | 向量嵌入和检索 |
| `useConfig()` | `PluginConfig` | 插件配置 |
| `useDatabase()` | `Database` | 数据库查询 |
| `useScheduler()` | `Scheduler` | 任务调度 |
| `useLLM()` | `PromptExecutor` | LLM 调用 |
| `useHttp()` | `HttpClient` | HTTP 客户端 |
| `useServer()` | `Server` | HTTP 路由注册 |

这些函数的实现基于 Kotlin 协程上下文（`currentCoroutineContext()`）查找 `PluginContextElement` 或 `MetaElement`。KSP 生成的扩展代码自动注入相应上下文。

## KSP 自动生成文件清单

KSP 处理器（`KspAnnotationProcessor`）为注解插件生成以下文件：

| 生成文件 | 说明 | 生成条件 |
|:---|:---|:---|
| `GeneratedPlugin.kt` | 插件主类，继承 `AgentPlugin`，包含 `@PluginDefinition` 和生命周期方法 | 始终生成 |
| `GeneratedRoute_<函数名>.kt` | Route 扩展类 | 每个 `@Route` 函数 |
| `GeneratedCmd_<命令名>.kt` | Cmd 扩展类（`SlashCmdExtension`） | 每个 `@Cmd` 函数 |
| `GeneratedPassive_<函数名>.kt` | Passive 扩展类 | 每个 `@Passive` 函数 |
| `GeneratedPassive_default.kt` | 默认被动扩展（仅注册工具集，无 chain handler） | 仅有 `@LLMTool` 函数而无其他扩展时 |
| `GeneratedToolSet_<set名>.kt` | 工具集类，继承 `MetaToolSet` | 每个 `set` 参数值对应的工具组合 |
| `META-INF/services/org.pf4j.Extension` | PF4J 扩展注册文件 | 有任意扩展时生成 |

## 参数插槽验证

KSP 处理器对函数参数进行严格的插槽匹配验证：

**Route / Passive 函数插槽（按顺序可选）：**

| 顺序 | 类型 | 说明 |
|:---|:---|:---|
| 1 | `meta: Meta` | 消息元数据 |

**Cmd 函数插槽（按顺序可选）：**

| 顺序 | 类型 | 说明 |
|:---|:---|:---|
| 1 | `meta: Meta` | 消息元数据 |
| 2 | `args: List<String>` | 命令参数列表 |

参数声明规则：
- 可声明少于插槽数的参数（取插槽的前缀）
- 不可声明多于插槽数的参数（编译错误）
- 参数类型必须与插槽类型严格匹配，顺序不可打乱

## 真实示例：lolisuki 插件完整流程

```kotlin
@file:Definition(pluginId = "lolisuki", version = "0.0.1", description = "二次元涩图插件")
package uesugi.plugin

// 1. LLM 判定用户意图 → 匹配 @Route
@Route(path = "REQUEST_R18_IMAGE", method = "<LLM分类规则>", toolSets = ["lolisuki"])
suspend fun lolisukiRoute(meta: Meta) {
    val image = getImage(meta)  // 从 lolisuki.cn API 搜索下载图片

    // 2. 调用 meta.sendAgent() 让 Agent 携带自定义工具发消息
    meta.sendAgent(
        input = "加入群聊天，你已经获取到了一张涩图，调用工具发送",
        ToolSetBuilder { tool { ImageTool(image, groupId, it, state) } }
            + Feature(PSFeature.GRAB or PSFeature.FALLBACK),
    ) {
        runCompletion { /* fallback: 直接发送图片 */ }
        callCompletion { /* fallback: 直接发送图片 */ }
        callFallback { /* fallback: 直接发送图片 */ }
        scope
    }
}

// 3. 声明为 default 工具集的工具（可在 @Route toolSets 之外独立存在）
@LLMTool(set = "lolisuki")
@LLMDesc("发送一张涩图")
suspend fun sendSexImage(): String { ... }

// 4. 使用上下文函数访问系统能力
suspend fun getImage(meta: Meta): ByteArray? {
    val database = useDatabase()     // 查询聊天历史
    val llm = useLLM()               // 调用 LLM 提取关键词
    val http = useHttp()             // 调用 lolisuki.cn HTTP API

    val history = database.getLatestHistory(meta.botId, meta.groupId, 10, 1.days)
    // LLM 从历史中提取图片关键词 → 调用 API 搜索 → 下载图片
    // ...
}
```
