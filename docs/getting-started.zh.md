# 快速安装

## 前置要求

- **Node.js** 18+ — 安装方法见 [附录：环境安装](appendix.md#nodejs)
- **NapCat** — QQ 接入服务，安装方法见 [附录：环境安装](appendix.md#napcat)
- **LLM API Key** — 需至少一个 LLM 提供商的 API Key

## 脚本安装（推荐）

脚本会自动检查并安装 Node.js 环境。

=== "Linux / macOS"

    ```bash
    curl -fsSL https://raw.githubusercontent.com/spcookie/erii-distribution/refs/heads/master/scripts/install.sh | bash
    ```

=== "Windows PowerShell"

    ```powershell
    irm https://raw.githubusercontent.com/spcookie/erii-distribution/refs/heads/master/scripts/install.ps1 | iex
    ```

## 全局安装

如果你已安装 Node.js，可直接全局安装：

```bash
npm install -g @spcookie/erii
erii setup
```

## 项目创建

在已有项目中创建 erii 子项目，适合需要自定义项目结构的高级用户。支持多种包管理器：

=== "npm"

    ```bash
    npm create @spcookie/erii
    ```

=== "pnpm"

    ```bash
    pnpm create @spcookie/erii
    ```

=== "yarn"

    ```bash
    yarn create @spcookie/erii
    ```

=== "bun"

    ```bash
    bun create @spcookie/erii
    ```

安装完成后进入项目目录：

```bash
cd erii
```

## 初始化配置

安装完成后，运行交互式配置向导：

```bash
erii setup
```

向导依次引导配置 AI 模型、可选工具、QQ 连接和群组设置。

配置完成后，项目目录会生成 `conf/application.conf`、`conf/.env.local` 等配置文件。

## 下一步

[启动服务](server/start.md)
