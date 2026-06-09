# Local Development Environment

## Requirements

| Dependency | Version | Notes |
|:---|:---|:---|
| **JDK** | 17+ | Kotlin/JVM backend development |
| **Go** | 1.21+ | CLI tool development (optional) |
| **Node.js** | 18+ | npm distribution related (optional) |
| **Gradle** | 9.2+ | Build tool (via wrapper) |

## Clone the Project

```bash
git clone --recurse-submodules https://github.com/spcookie/erii.git
cd erii
```

!!! warning "Note on submodules"
    erii uses Git Submodules to manage `erii-distribution` and `erii-docs`. Always use `--recurse-submodules` when cloning.

## Project Structure

```
erii/
├── erii-common/           # Common module (EventBus, ChatToolSet, data models)
├── erii-core/             # Core module (Ktor app, Agent, state system)
├── erii-spi/              # SPI interfaces
│   ├── erii-spi-core/     # Extension interface definitions (AgentExtension, etc.)
│   └── erii-spi-annotation/  # Annotations + KSP processor
├── erii-plugin-gradle/    # Plugin build Gradle convention plugin
├── erii-plugins/          # Built-in plugins (Git Submodule)
├── erii-cli/              # CLI tool (Go)
├── erii-distribution/     # npm distribution (Git Submodule)
└── erii-docs/             # Documentation
```

## Build and Run

| Command | Description |
|:---|:---|
| `./gradlew compileKotlin` | Compile Kotlin code |
| `./gradlew build` | Build the entire project |
| `./gradlew run` | Start the development server |
| `./gradlew buildFatJar` | Build an executable fat JAR with all dependencies |
| `./gradlew buildImage` | Build Docker image |
| `./gradlew test` | Run all tests |
| `./gradlew :erii-core:test --tests "com.example.MyTest"` | Run a single test |
| `cd erii-plugins && ./gradlew buildAllPlugins` | Build all plugins |
| `cd erii-plugins && ./gradlew :<name>:build` | Build a single plugin |
| `cd erii-cli && go build .` | Build the CLI tool |

## Starting the Dev Server

```bash
./gradlew run
```

The development server listens on `http://localhost:8080` by default, with logs output to the console.
