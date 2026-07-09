# 构建与调试

## 构建插件

### 构建单个插件

```bash
cd erii-plugins
./gradlew :my-plugin:build
```

构建过程：
1. KSP 注解处理器执行（若有注解标注的函数，生成 `Generated*` 类）
2. Kotlin 编译（包含生成的代码）
3. PF4J 插件 zip 打包（产出在 `build/plugin/` 目录）

### 构建全部插件

```bash
cd erii-plugins
./gradlew buildAllPlugins
```

`buildAllPlugins` 是 `erii-plugins/build.gradle.kts` 中定义的聚合任务，遍历所有已注册的子项目执行构建。

### 构建产物

```
erii-plugins/my-plugin/
└── build/
    ├── plugin/
    │   └── my-plugin-0.0.1.zip     ← 可直接部署的插件包
    ├── generated/
    │   └── ksp/                     ← KSP 生成的源码
    └── classes/
        └── kotlin/
            └── main/                ← 编译后的 .class 文件
```

### 部署测试

将构建产物复制到 erii 项目的 `plugins/` 目录：

```bash
cp erii-plugins/my-plugin/build/plugin/my-plugin-0.0.1.zip /path/to/erii/plugins/
```

然后执行：

```bash
erii reload   # 初始化插件配置
erii refresh  # 通知后端刷新缓存（若服务在运行）
```

或直接重启服务加载新插件：

```bash
erii server restart
```

## 调试方法

### 方法一：挂载到开发服务器

1. 在 `erii-plugins/` 中开发插件
2. 启动 erii 开发服务器（会自动加载 `erii-plugins/plugins/` 目录下的本地构建产物）：

    ```bash
    cd /path/to/erii
    ./gradlew run
    ```

3. 在 IDE（IntelliJ IDEA）中设置断点于插件代码
4. 通过 NapCat/OneBot 在群内发送消息触发插件逻辑
5. IDE 在断点处暂停，可检查变量状态、单步执行

??? tip "开发服务器插件加载路径"
    通过 `./gradlew run` 启动时，PF4J 加载 `plugins/` 目录下的 zip 包。开发时需先用 `./gradlew :my-plugin:build` 生成 zip，或手动将 `build/plugin/` 产物链接到 `plugins/` 目录。

### 方法二：日志调试

使用 KotlinLogging 在关键路径输出日志：

```kotlin
import io.github.oshai.kotlinlogging.KotlinLogging

private val log = KotlinLogging.logger {}

class MyExtension : PassiveExtension<MyPlugin> {
    override fun onLoad(context: PluginContext) {
        log.info { "Plugin loaded: ${context.defined.name}" }
        log.debug { "Bot ID: $botId, Group ID: $groupId" }

        context.chain { meta ->
            log.info { "Received message from ${meta.senderId}: ${meta.input}" }
            // 处理逻辑...
            log.debug { "Processing completed, result: $result" }
        }
    }

    override fun onUnload() {
        log.info { "Plugin unloaded" }
    }
}
```

日志级别：
- `log.info { }` — 一般信息（插件加载、请求处理）
- `log.debug { }` — 调试细节（变量值、中间状态）
- `log.warn { }` — 警告信息（降级处理、非致命错误）
- `log.error { }` — 错误信息（异常堆栈跟踪）

查看日志：

```bash
erii log
```

在开发模式（`ERII_START_MODE=CLI`）下，日志直接输出到控制台。

### 方法三：KSP 生成代码检查

当注解开发模式下功能异常时，检查 KSP 生成的代码以定位问题：

```
erii-plugins/my-plugin/build/generated/ksp/main/kotlin/
├── GeneratedPlugin.kt                # 插件主类
├── GeneratedRoute_<函数名>.kt        # Route 扩展
├── GeneratedCmd_<命令名>.kt          # Cmd 扩展
├── GeneratedPassive_<函数名>.kt      # Passive 扩展
├── GeneratedToolSet_<set名>.kt       # 工具集
└── META-INF/services/
    └── org.pf4j.Extension            # PF4J 扩展注册
```

常见问题检查：
- 函数是否为 `private`（注解函数不能是 private）
- 函数参数是否与插槽类型匹配（`meta: Meta`、`args: List<String>`）
- `@file:Definition` 是否存在于文件中
- 工具集名称（`set` 参数）是否在各注解间一致
- `@OnLoad`/@OnUnload 的名称引用是否正确

KSP 在编译时会输出错误信息：

```
error: @Route function 'myFunc' must not be private.
error: @Cmd function 'myCmd' declares 3 parameter(s), but only 2 slot(s) are available.
error: @OnLoad/OnUnload("missing") not found, referenced by 'myRoute'
```

### 方法四：通过 `erii stats` 确认插件状态

```bash
erii stats
```

在 stats TUI 的插件面板中查看：

| 指标 | 含义 |
|:---|:---|
| `totalExtensions` | 扩展总数（应等于你的插件扩展类数量） |
| `cmdExtensions` | 命令扩展数量 |
| `routeExtensions` | 路由扩展数量 |
| `passiveExtensions` | 被动扩展数量 |

若插件未出现在 stats 中，检查：
1. 插件 zip 是否在 `plugins/` 目录中
2. PF4J `plugin.properties` 中的 `plugin.id` 和 `plugin.version` 是否正确
3. `META-INF/services/org.pf4j.Extension` 文件是否列出了所有扩展全限定名
4. 服务端日志中是否有 PF4J 加载错误信息

### 方法五：单元测试

```kotlin
// erii-plugins/my-plugin/src/test/kotlin/myplugin/MyPluginTest.kt
import kotlin.test.Test
import kotlin.test.assertEquals

class MyPluginTest {
    @Test
    fun testCoreLogic() {
        val result = myCoreFunction("input")
        assertEquals("expected", result)
    }
}
```

运行测试：

```bash
./gradlew :erii-plugins:my-plugin:test
```

或运行单个测试类：

```bash
./gradlew :erii-plugins:my-plugin:test --tests "myplugin.MyPluginTest.testCoreLogic"
```

## 开发技巧

### 热加载开发流程

```bash
# 1. 修改插件代码
# 2. 重新构建
cd erii-plugins && ./gradlew :my-plugin:build

# 3. 复制到 plugins 目录
cp build/plugin/my-plugin-0.0.1.zip /path/to/erii/plugins/

# 4. 热重载（无需重启服务）
erii reload
erii refresh
```

??? warning "热加载限制"
    若插件修改涉及以下变更，需要重启服务而非 `reload`：
    - 新增了外部依赖 jar（需要更新 classpath）
    - 修改了 `@PluginDefinition` 的 `requires` 或 `dependencies`
    - 插件 zip 包内的 `lib/` 目录结构变化

### 快速验证工具函数的 TUI 客户端

```bash
erii chat
```

`chat` 命令启动一个基于 Bubble Tea 的 TUI 聊天客户端，通过模拟 OneBot WebSocket 桥接与本地 erii 服务通信。可在客户端中直接发送 `/命令` 或触发路由来测试插件功能。

### Web 远程控制台

```bash
erii web --port 9527
```

在浏览器中打开 `http://localhost:9527/?token=<生成的token>` 获得基于 xterm.js 的 Web 终端，可在浏览器中运行 erii TUI 的所有功能。支持 `--host 0.0.0.0` 暴露到局域网。

### 常见调试清单

| 问题 | 检查项 |
|:---|:---|
| 插件未加载 | PF4J 日志、zip 是否在 `plugins/` 目录、`plugin.properties` |
| 扩展未注册 | `META-INF/services/org.pf4j.Extension` 文件内容、扩展类是否标注 `@Extension` |
| 命令无响应 | `@Cmd` 的 `name` 是否与用户输入匹配、`alias` 列表是否正确 |
| 路由未匹配 | LLM RoutingAgent 的分类提示词（`method` 参数）是否清晰准确 |
| 工具不可用 | 工具集名称在 `@Route`/`@Cmd`/`@Passive` 的 `toolSets` 参数与 `@LLMTool` 的 `set` 参数是否一致 |
| 上下文函数报错 | 是否在正确的协程上下文中调用（chain handler 或 withPluginContext 块内） |
| 配置未生效 | 执行 `erii refresh` 清除缓存、检查配置优先级 |
