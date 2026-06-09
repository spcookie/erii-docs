# Class-based Development

Suitable for complex plugins that require fine-grained control over lifecycle and extension behavior.

## 1. Create Plugin Project

Create a new directory under `erii-plugins/`:

```
erii-plugins/my-plugin/
├── build.gradle.kts
└── src/main/kotlin/
    └── myplugin/
        ├── MyPlugin.kt
        └── MyExtension.kt
```

## 2. Register Module

Add to `erii-plugins/settings.gradle.kts`:

```kotlin
include("my-plugin")
```

## 3. Configure Build

```kotlin
// erii-plugins/my-plugin/build.gradle.kts
plugins {
    id("uesugi.erii-plugin")
}

version = "0.0.1"
```

## 4. Define Plugin

```kotlin
@PluginDefinition(
    pluginId = "my-plugin",
    version = "0.0.1",
    description = "My plugin",
    provider = "your-name"
)
class MyPlugin : AgentPlugin()
```

## 5. Implement Extensions

=== "AgentExtension"

    ```kotlin
    class MyAgentExt : AgentExtension<MyPlugin> {
        override fun onLoad(context: PluginContext) {
            // Initialize: register scheduled tasks, tools, routes, etc.
            context.scheduler.scheduleRecurrently("my-task", "0 */1 * * *") {
                // Runs every hour
            }
        }

        override fun onUnload() {
            // Clean up resources
        }
    }
    ```

=== "RouteExtension"

    ```kotlin
    class MyRouteExt : RouteExtension<MyPlugin> {
        // LLM matches this extension based on matcher key and value
        override val matcher: Pair<String, String>
            get() = "image" to "Generate or search images"
    }
    ```

=== "CmdExtension"

    ```kotlin
    class MyCmdExt : SlashCmdExtension<MyPlugin> {
        override val cmd: String = "hello"
        override val alias: List<String> = listOf("hi", "hey")

        override fun onLoad(context: PluginContext) {
            context.chain { meta ->
                // Handle command logic
            }
        }
    }
    ```

=== "PassiveExtension"

    ```kotlin
    class MyPassiveExt : PassiveExtension<MyPlugin> {
        override fun onLoad(context: PluginContext) {
            context.scheduler.scheduleRecurrently("bg-task", "*/30 * * * *") {
                // Background periodic task
            }
        }
    }
    ```
