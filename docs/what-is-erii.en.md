# What is erii

erii is an AI bot that participates in group chats. It is not merely a message reply system — it behaves like a real group member, with its own emotions, memory, and personality.

## What It Can Do

**Has emotions, reads the room**

erii's emotions shift with the conversation. Lively when the group is buzzing, reserved when things get serious — never out of place.

**Remembers, gets better over time**

erii remembers what people say and what they like. The longer you chat, the more it understands the group and matches its vibe.

**Proactive, not just reactive**

When the group goes quiet, erii starts conversations. When something catches its interest, it jumps in — just like a real group member.

**Follows rules, stays in line**

Set rules with simple Markdown files. Different groups can have different codes of conduct, and erii follows them strictly.

**Reminders and summaries**

Schedule reminders and erii will notify you on time. It can also automatically summarize conversations you missed.

## Technical Features

| Feature          | Description                                                              |
|:-----------------|:-------------------------------------------------------------------------|
| Multi-LLM        | OpenAI and Anthropic compatible APIs                                     |
| MCP Tools        | Extend AI Agent capabilities via Model Context Protocol                  |
| Plugin System    | Add new features through plugins: TTS, image generation, music search, etc. |
| Multi-Bot        | Configure multiple bots with different personalities for different groups |
| CLI Tool         | Command-line tools for configuration and data management                 |

## How It Works

erii connects to QQ via a OneBot V11 server. Incoming messages are processed by AI models, constrained by rule files, with all data stored locally — no cloud service required.

Plugins extend its capabilities — from sending memes to generating images, from TTS to virtual pet games.

## Next Steps

- [Getting Started](getting-started.md): Installation and basic configuration
- [Server Management](server/start.md): Start and operations
- [CLI Commands](cli/setup.md): Complete command-line reference
- [Plugins](plugins/install.md): Extend erii's capabilities
