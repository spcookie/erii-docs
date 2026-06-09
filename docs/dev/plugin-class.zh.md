# 类开发模式

类开发模式通过直接继承接口和手写实现类来完成插件开发。适合逻辑复杂、需要精细控制生命周期、混合多种扩展类型的插件。

以下以 erii 内置的 `animal`（虚拟宠物养成）插件为真实示例进行说明。

## 1. 创建插件项目

在 `erii-plugins/` 下创建新目录：

```
erii-plugins/my-plugin/
├── build.gradle.kts
└── src/main/kotlin/
    └── myplugin/
        ├── MyPlugin.kt
        └── MyExtension.kt
```

## 2. 注册模块

在 `erii-plugins/settings.gradle.kts` 中添加：

```kotlin
include("my-plugin")
```

## 3. 配置构建脚本

```kotlin
// erii-plugins/my-plugin/build.gradle.kts
plugins {
    id("uesugi.erii-plugin")
}

version = "0.0.1"
```

`uesugi.erii-plugin` 是 erii 提供的 Gradle 约定插件，自动配置了 PF4J 打包、Kotlin 编译、依赖管理等功能。

## 4. 定义插件主类

使用 `@PluginDefinition` 类注解标注插件主类，继承 `AgentPlugin()`：

```kotlin
package uesugi.plugin.animal

import uesugi.spi.AgentPlugin
import uesugi.spi.PluginDefinition

@PluginDefinition(
    pluginId = "animal",
    version = "0.0.1",
    description = "虚拟宠物养成插件"
)
class Animal : AgentPlugin()
```

`@PluginDefinition` 的参数：

| 参数 | 说明 | 必需 |
|:---|:---|:---|
| `pluginId` | 插件唯一标识符，PF4J 通过此 ID 管理插件 | 是 |
| `version` | 语义化版本号 | 是 |
| `requires` | 依赖的其他插件 ID | 否 |
| `dependencies` | 外部依赖声明 | 否 |
| `description` | 插件描述文本 | 否 |
| `provider` | 插件作者/组织 | 否 |
| `license` | 许可证类型 | 否 |

编译时 `PluginDefinitionProcessor`（javax.annotation.processing）读取此注解并生成 `META-INF/plugin.properties` 文件。

## 5. 实现扩展

### 5.1 同时实现多种扩展接口

`animal` 插件的 `AnimalExtension` 同时实现了 `PassiveExtension` 和 `CmdExtension` 两种接口，还扮演了 `RouteExtension` 的角色（通过 `context.chain` 注册）：

```kotlin
package uesugi.plugin.animal

import org.pf4j.Extension
import uesugi.spi.*

@Extension
class AnimalExtension : PassiveExtension<Animal>,
    CmdExtension<AnimalContext, AnimalArgParser, Animal> {

    override val cmd: String = "animal"

    private lateinit var context: PluginContext
    private lateinit var store: AnimalStore
    private lateinit var service: AnimalService
    private lateinit var dailyTaskService: DailyTaskService
    private lateinit var htmlRenderer: AnimalHtmlRenderer
    private val scope = CoroutineScope(Dispatchers.IO + SupervisorJob())
    private val serverPort = 8888
```

注意：扩展类必须标注 `@Extension`（PF4J 的 `org.pf4j.Extension` 注解），PF4J 通过 SPI 服务发现机制（`META-INF/services/org.pf4j.Extension`）自动加载。

### 5.2 CmdExtension 的三个泛型参数

```kotlin
interface CmdExtension<Context, Arg : ArgParserHolder<Context>, Plugin : AgentPlugin>
    : AgentExtension<Plugin>
```

| 泛型参数 | 说明 | animal 示例 |
|:---|:---|:---|
| `Context` | 命令执行上下文类型 | `AnimalContext` |
| `Arg` | Clikt 参数解析器类型（继承 `ArgParserHolder<Context>`） | `AnimalArgParser` |
| `Plugin` | 插件主类类型 | `Animal` |

`ArgParserHolder<Context>` 继承 `CliktCommand`，可利用 Clikt 库的参数声明式定义子命令和选项。在 `Meta.parser(context)` 中自动解析 `/animal <子命令> <参数>`。

### 5.3 onLoad() — 初始化

```kotlin
override fun onLoad(context: PluginContext) {
    this.context = context

    // 1. 初始化持久化存储
    store = AnimalStore(context.kv)

    // 2. 初始化业务服务层
    service = AnimalService(store)

    // 3. 注册日常定时任务
    dailyTaskService = DailyTaskService(store, service, context.scheduler)
    dailyTaskService.startDailyTasks()

    // 4. 注册自定义 HTTP 路由（通过 context.server.route {}）
    htmlRenderer = AnimalHtmlRenderer(store, context)
    htmlRenderer.registerHtmlRoutes()

    // 5. 注册命令处理器
    registerCommandHandler()

    // 6. 注册 LLM 工具集
    registerTools()

    log.info { "AnimalExtension loaded" }
}
```

#### 注册命令处理链

```kotlin
private fun registerCommandHandler() {
    context.chain { meta ->
        AnimalCommandHandler(
            store = store,
            service = service,
            serverPort = serverPort,
            serverBasePath = context.server.url.pathSegments.joinToString("/", "/"),
            scope = scope
        ).handleWithError(meta) { ctx ->
            meta.parser(ctx)  // Clikt 参数解析，匹配合适的子命令
        }
    }
}
```

`context.chain { meta -> ... }` 注册一个消息处理链：
- 每次群组收到消息时被调用
- `meta.input` 包含用户消息文本
- `meta.parser(ctx)` 匹配 CmdExtension 的 `cmd`/`alias` 并调用 Clikt 解析参数

#### 注册 LLM 工具集

```kotlin
private fun registerTools() {
    context.tool {
        {
            AnimalToolSet(
                store,
                service,
                serverPort,
                context.server.url.pathSegments.joinToString("/", "/"),
            )
        }
    }
}
```

`context.tool { { ... } }` 注册一个返回 `MetaToolSetCreator` 的工厂函数。Agent 在 Function Calling 时可调用 `AnimalToolSet` 中标注了 `@Tool` 的方法。

### 5.4 ToolSet 实现示例

```kotlin
class AnimalToolSet(
    private val store: AnimalStore,
    private val service: AnimalService,
    private val serverPort: Int,
    private val serverBasePath: String,
) : MetaToolSet {

    // 创建命令执行上下文
    private fun createAnimalContext(): AnimalContext {
        return AnimalContextFactory.createFromMeta(
            meta = MetaToolSet.meta,
            store = store, service = service,
            serverPort = serverPort, serverBasePath = serverBasePath
        )
    }

    // 通用命令执行
    private fun runCommand(argv: List<String>): String? {
        return try {
            val ctx = createAnimalContext()
            val parser = AnimalArgParser()
            parser.init(MetaToolSet.meta, ctx)
            parser.main(argv)
            null
        } catch (e: Exception) {
            log.error(e) { "Failed to run command" }
            "执行失败：${e.message}"
        }
    }

    @Tool
    @LLMDescription("注册用户，获取一只随机宠物")
    fun registerAnimal(): String? = runCommand(listOf("register"))

    @Tool
    @LLMDescription("查看用户的宠物列表")
    fun listAnimals(): String? = runCommand(listOf("list"))

    @Tool
    @LLMDescription("使用金币抽宠物，100金币抽1次，1000金币抽10次")
    fun drawAnimal(count: Int = 1): String? = runCommand(listOf("draw", count.toString()))

    @Tool
    @LLMDescription("售卖宠物，需要宠物ID参数")
    fun sellAnimal(petId: Long): String? = runCommand(listOf("sell", petId.toString()))
}
```

`MetaToolSet` 通过 `MetaToolSet.meta` 静态属性获取当前消息的 `Meta` 对象。

### 5.5 自定义 HTTP 路由（Server.route）

```kotlin
// AnimalHtmlRenderer 中
fun registerHtmlRoutes() {
    context.server.route {
        get("/animal/{petId}") {
            // 渲染宠物详情 HTML 页面
        }
        get("/animal/farm") {
            // 渲染宠物农场 HTML 页面
        }
    }
}
```

`context.server.route {}` 使用 Ktor 的 Route DSL，完全等价于在 Ktor 模块中注册路由。

### 5.6 onUnload() — 清理

```kotlin
override fun onUnload() {
    dailyTaskService.stopDailyTasks()
    scope.cancel()  // 取消协程作用域
    log.info { "AnimalExtension unloaded" }
}
```

## 6. 手动注册 PF4J Extensions 服务文件

类开发模式下，需要在插件项目的 `src/main/resources/META-INF/services/org.pf4j.Extension` 文件中手动列出所有扩展类的全限定名：

```
uesugi.plugin.animal.AnimalExtension
```

注解模式下此文件由 KSP 自动生成。

## 完整开发流程总结

| 步骤 | 操作 | 文件位置 |
|:---|:---|:---|
| 1 | 创建插件目录 | `erii-plugins/my-plugin/` |
| 2 | 注册 Gradle 子项目 | `erii-plugins/settings.gradle.kts` |
| 3 | 配置构建 | `erii-plugins/my-plugin/build.gradle.kts` |
| 4 | 定义插件主类 | `@PluginDefinition class MyPlugin : AgentPlugin()` |
| 5 | 实现扩展 | 实现 `AgentExtension<T>` / `RouteExtension<T>` / `CmdExtension` / `PassiveExtension<T>` |
| 6 | 实现 onLoad() | 初始化存储、注册 chain/tool/scheduler/route |
| 7 | 实现 onUnload() | 清理资源、取消协程 |
| 8 | 添加 services 文件 | `META-INF/services/org.pf4j.Extension` |
| 9 | 构建 | `cd erii-plugins && ./gradlew :my-plugin:build` |
| 10 | 部署 | 将 `build/plugin/my-plugin-*.zip` 放入 `plugins/` 目录 |
| 11 | 重载 | `erii reload && erii refresh` |
