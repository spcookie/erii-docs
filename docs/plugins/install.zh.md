# 安装插件

erii 支持通过插件扩展功能。

## 内置插件

erii 自带以下插件：

| 插件 | 功能 |
|:---|:---|
| `speech` | 语音合成，将文字转为语音发送 |
| `lolisuki` | 二次元图片获取 |
| `net-ease-music` | 网易云音乐搜索和分享 |
| `qq-face` | QQ 表情匹配发送 |
| `seeddream` | AI 图片生成 |
| `rollpig` | 抽小猪收集游戏 |
| `animal` | 虚拟宠物养成 |

## 安装新插件

=== "npm 安装"

    ```bash
    npm install @作者名/erii-plugin-xxx
    ```

=== "手动安装"

    将插件 zip 包放入 `plugins/` 目录：

    ```bash
    cp my-plugin-0.0.1.zip /path/to/erii/plugins/
    ```

安装后运行 `erii reload` 使其生效。
