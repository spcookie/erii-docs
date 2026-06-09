# 附录：环境安装

## Node.js

erii 需要 Node.js 18 或更高版本。

=== "Windows"

    1. 访问 [Node.js 官网](https://nodejs.org/) 下载 LTS 版本
    2. 运行安装程序，按默认选项完成安装
    3. 打开 PowerShell 或命令提示符，验证安装：

    ```powershell
    node --version
    npm --version
    ```

=== "macOS"

    ```bash
    # 推荐使用 Homebrew 安装
    brew install node@18

    # 或从官网下载安装包
    # https://nodejs.org/
    ```

    验证安装：

    ```bash
    node --version
    npm --version
    ```

=== "Linux"

    ```bash
    # Ubuntu / Debian
    curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
    sudo apt-get install -y nodejs

    # CentOS / Fedora
    curl -fsSL https://rpm.nodesource.com/setup_18.x | sudo bash -
    sudo yum install -y nodejs
    ```

    验证安装：

    ```bash
    node --version
    npm --version
    ```

## NapCat

NapCat 是 QQ 的接入服务，让 erii 能够收发 QQ 消息。

### 安装 NapCat

1. 访问 [NapCat 官方文档](https://napneko.github.io/)
2. 根据你的系统选择对应的安装方式：
    - **Windows**：推荐使用一键安装包
    - **Linux**：推荐使用 Docker 部署
    - **macOS**：推荐使用 Docker 部署

### 获取 WebSocket 地址和 Token

启动 NapCat 后，在配置界面中找到：

- **WebSocket 地址**：通常为 `ws://127.0.0.1:3001`
- **Access Token**：在 NapCat 设置中生成

这两个信息在 erii setup 向导的第三步中需要填写。

### Docker 部署 NapCat（可选）

```bash
cd docker/napcat-docker
docker-compose up -d
```
