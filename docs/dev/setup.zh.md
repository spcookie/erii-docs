# 本地开发环境

erii 采用多语言混合工程结构：Kotlin/JVM 后端、Go CLI 工具、TypeScript npm 分发层。开发者需要根据目标组件准备对应环境。

## 环境要求

| 依赖 | 最低版本 | 用途 | 必需 |
|:---|:---|:---|:---|
| **JDK** | 17+ | Kotlin/JVM 后端核心、内嵌插件开发 | 是 |
| **Go** | 1.21+ | CLI 工具 (`erii-cli/`) 开发与构建 | 否（仅 CLI 开发） |
| **Node.js** | 18+ | npm 分发 (`erii-distribution/`) 管理 | 否（仅分发） |
| **Gradle** | 8.x (Wrapper) | 构建系统，通过 `gradlew` 自动下载 | 是 |

## 克隆项目

```bash
git clone --recurse-submodules https://github.com/spcookie/erii.git
cd erii
```

!!! warning "子模块必须初始化"
    erii 使用 Git Submodule 管理 `erii-distribution`（npm 分发）和 `erii-docs`（文档）。克隆时务必使用 `--recurse-submodules` 标志。若已克隆但缺少子模块，执行：
    ```bash
    git submodule update --init --recursive
    ```

## 完整项目结构

```
erii/
├── erii-common/                  # 公共模块
│   └── src/main/kotlin/uesugi/common/
│       ├── data/                 # 数据模型（HistoryRecord, HistoryTable 等）
│       ├── event/                # 事件系统（PSFeature, ProactiveSpeakFeature）
│       ├── IBotManage.kt         # Bot 管理接口
│       ├── LLMProviderChoice.kt  # LLM 提供商选择器
│       └── toolkit/              # 工具集（ConfigHolder, BrowserScraperHolder）
├── erii-core/                    # 核心服务端
│   └── src/main/kotlin/uesugi/
│       ├── Application.kt        # 应用入口，EngineMain.main() → module()
│       ├── AnsiColor.kt          # 控制台 ANSI 彩色输出
│       ├── cli/IPC.kt            # IPC mmap 通信（Kotlin 端）
│       ├── config/               # 配置系统
│       │   ├── ConfigHolderImpl.kt  # HOCON 配置持有器实现
│       │   ├── DatabaseConfig.kt    # H2 + HikariCP 数据库配置
│       │   ├── JobRunrConfig.kt     # JobRunr 任务调度配置
│       │   ├── LLMFactory.kt        # LLM 客户端工厂
│       │   ├── Migration.kt         # 数据库表迁移
│       │   └── ...
│       ├── core/                 # 核心业务逻辑
│       │   ├── agent/            # Agent 系统（BotAgent, Tools, Prompts, Contexts）
│       │   ├── bot/              # Bot 管理与角色
│       │   ├── chat/             # Chat Bridge（WebSocket ↔ OneBot 适配）
│       │   ├── component/browser/ # BrowserScraper（Playwright 集成）
│       │   └── state/            # 状态管理（emotion, flow, volition, memory, meme, summary）
│       └── server/               # Ktor 路由与中间件
├── erii-spi/                     # SPI 服务提供接口
│   ├── erii-spi-core/            # 扩展接口定义
│   │   └── src/main/kotlin/uesugi/spi/
│   │       ├── Ext.kt            # 核心接口（AgentExtension, CmdExtension, RouteExtension,
│   │       │                       PassiveExtension, PluginContext, Scheduler, Meta, Database 等）
│   │       ├── PluginDefinition.kt  # @PluginDefinition 注解（类模式）
│   │       └── AgentPlugin.kt       # AgentPlugin 基类
│   └── erii-spi-annotation/      # 注解 + KSP 代码生成器
│       └── src/main/kotlin/uesugi/spi/annotation/
│           ├── Definition.kt     # @file:Definition 文件级注解
│           ├── Route.kt          # @Route 注解
│           ├── Cmd.kt            # @Cmd 注解
│           ├── Passive.kt        # @Passive 注解
│           ├── LLMTool.kt        # @LLMTool 注解
│           ├── LLMDesc.kt        # @LLMDesc 注解
│           ├── Plugin.kt         # @Plugin 类级注解
│           ├── OnLoad.kt        # @OnLoad 生命周期
│           ├── OnStart.kt       # @OnStart 生命周期
│           ├── OnStop.kt        # @OnStop 生命周期
│           ├── OnUnload.kt      # @OnUnload 生命周期
│           ├── ContextDsl.kt     # 上下文 DSL（useMem, useKv, useLLM 等）
│           ├── MetaDsl.kt        # 元数据 DSL（useMeta, useToolMeta）
│           └── processor/        # KSP 注解处理器
│               ├── KspAnnotationProcessor.kt         # 核心处理器
│               └── KspAnnotationProcessorProvider.kt  # 处理器提供者
├── erii-plugin-gradle/           # 插件构建 Gradle 约定插件
│   └── uesugi.erii-plugin.gradle.kts
├── erii-plugins/                 # 内置插件（Git Submodule）
│   ├── settings.gradle.kts       # 插件子项目注册
│   ├── build.gradle.kts          # buildAllPlugins task
│   ├── speech/                   # TTS 语音合成
│   ├── lolisuki/                 # 二次元涩图（注解模式示例）
│   ├── net-ease-music/           # 网易云音乐（注解模式）
│   ├── qq-face/                  # QQ 表情
│   ├── seeddream/                # AI 图片生成
│   ├── rollpig/                  # 抽小猪游戏
│   ├── animal/                   # 虚拟宠物养成（类模式示例）
│   └── chat-heatmap/             # 聊天热力图
├── erii-onebot11/                # OneBot v11 协议 SDK
├── erii-cli/                     # CLI 工具（Go）
│   ├── cmd/                      # 命令定义（Cobra 子命令）
│   │   ├── root.go               # 根命令 + 全局标志
│   │   ├── server.go / chat.go / config.go / config_cmd.go
│   │   ├── setup.go / manage.go / stats.go / reload.go / refresh.go / web.go
│   ├── internal/                 # 内部实现
│   │   ├── api/                  # HTTP 客户端（REST API + WebSocket 桥接）
│   │   ├── config/               # 配置读写（setter, tree 解析合并）
│   │   ├── ipc/                  # IPC mmap 通信（Go 端，含 Unix/Windows 平台适配）
│   │   ├── path/                 # 路径解析（findDirUpward 自动发现）
│   │   ├── tui/                  # Bubble Tea TUI 界面
│   │   │   ├── chatui/           # 聊天 TUI
│   │   │   ├── config/           # 配置 TUI
│   │   │   ├── manage/           # 数据管理 TUI
│   │   │   ├── setup/            # 安装向导 TUI
│   │   │   ├── stats/            # 统计 TUI
│   │   │   ├── components/       # 共享组件（连接错误、主菜单、插件浏览器等）
│   │   │   └── style/            # 主题和配色
│   │   ├── version/              # 版本信息
│   │   └── web/                  # Web 控制台（xterm.js）
│   ├── main.go                   # 入口
│   ├── go.mod / go.sum           # Go 依赖
│   └── magefile.go               # Mage 构建脚本
├── erii-distribution/            # npm 分发（Git Submodule）
├── erii-docs/                    # MkDocs 文档项目
├── build.gradle.kts              # 根构建脚本
├── settings.gradle.kts           # 根项目注册
├── gradle.properties             # Gradle 属性
└── docker-compose.yml            # Docker 编排
```

## 构建命令速查

| 命令 | 说明 |
|:---|:---|
| `./gradlew compileKotlin` | 编译所有 Kotlin 源码 |
| `./gradlew build` | 完整构建（编译 + 测试 + 打包） |
| `./gradlew run` | 启动 erii 开发服务器（`io.ktor.server.netty.EngineMain`） |
| `./gradlew buildFatJar` | 构建可执行 Fat JAR（含所有依赖） |
| `./gradlew buildImage` | 构建 Docker 镜像 |
| `./gradlew test` | 运行所有测试 |
| `./gradlew :erii-core:test --tests "com.example.MyTest"` | 运行单个测试类 |
| `cd erii-plugins && ./gradlew buildAllPlugins` | 构建全部插件（生成 zip 包到各插件的 `build/plugin/`） |
| `cd erii-plugins && ./gradlew :<name>:build` | 构建单个插件 |
| `cd erii-cli && go build .` | 编译 CLI 工具为本地可执行文件 |

## 启动开发服务器

```bash
./gradlew run
```

开发服务器默认监听 `http://localhost:8080`，日志输出到控制台。若使用 `erii-cli` 启动服务，会通过 JDK 检测、JVM 参数构建、PID 写入等完整流程。

## IDE 配置建议

- **IntelliJ IDEA Ultimate**：直接导入 `build.gradle.kts` 作为 Gradle 项目，KSP 注解处理自动启用
- **GoLand / VS Code（Go 插件）**：打开 `erii-cli/` 目录进行 Go 开发
- KSP 生成的代码位于 `build/generated/ksp/` 目录下，需在 IDE 中标记为 Generated Sources Root

## Gradle 属性

`gradle.properties` 中可配置的关键属性：

```properties
# Kotlin 版本
kotlin.code.style=official

# Ktor 版本
ktor.version=3.x.x

# PF4J 版本
pf4j.version=3.15+
```
