# Building and Debugging

## Building Plugins

```bash
cd erii-plugins

# Build a single plugin
./gradlew :my-plugin:build

# Build all plugins
./gradlew buildAllPlugins
```

Plugin build artifacts are located in the `build/plugin/` directory in zip format. Place them in your erii project's `plugins/` directory to load.

## Debugging Methods

### Method 1: Attach to Dev Server

1. Start the erii development server:

    ```bash
    ./gradlew run
    ```

2. Set breakpoints in your IDE, then trigger plugin logic by sending messages through NapCat.

### Method 2: Log Debugging

```kotlin
import io.github.oshai.kotlinlogging.KotlinLogging

private val log = KotlinLogging.logger {}

// Use in extensions
log.info { "Plugin loaded: $name" }
log.debug { "Processing message from ${meta.senderId}" }
log.error { "Failed to process: ${e.message}" }
```

### Method 3: Unit Testing

```kotlin
class MyPluginTest {
    @Test
    fun testPluginLogic() {
        // Test plugin core logic
    }
}
```

Run tests:

```bash
./gradlew :erii-plugins:my-plugin:test
```

## Development Tips

- :material-reload: After modifying plugins, run `erii reload` to hot-reload without restarting the server
- :material-file-eye: View plugin logs: `erii log`
- :material-state-machine: Check plugin load status via `erii stats`
