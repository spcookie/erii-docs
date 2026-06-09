# What is erii

erii is an AI group chat bot framework built on Kotlin and Ktor. It is not merely a message reply system, but an intelligent agent with comprehensive cognitive capabilities.

erii integrates emotion perception, memory storage, and personality modeling. Its emotional state adapts dynamically to conversation content, with linguistic style adjusting synchronously. It employs an active perception architecture rather than a passive command-response model: by analyzing group atmosphere, it engages or remains silent at appropriate moments, and proactively initiates topics when the group becomes inactive. Through sustained interaction, erii memorizes member preferences and acquires group-specific terminology and expressions.

## Core Systems

<div class="grid cards" markdown>

-   :material-emoticon-outline:{ .lg .middle } __Emotion System__

    ---

    PAD three-dimensional emotion model (Pleasure, Arousal, Dominance). Real-time emotional adjustment based on conversation content, influencing reply tone, emoji usage, and linguistic style.

-   :material-brain:{ .lg .middle } __Memory System__

    ---

    Multi-dimensional memory (user profiles, preferences, facts, summaries). Semantic retrieval based on vector similarity across three levels: global, group, and member.

-   :material-waves:{ .lg .middle } __Flow System__

    ---

    0-100 flow value quantifying conversational engagement. Rapid entry into focused state upon matching topics of interest, with built-in overheat protection.

-   :material-lightning-bolt:{ .lg .middle } __Volition System__

    ---

    Proactive speech impulse calculated from multi-dimensional factors including emotion, flow, and keywords. Three modes: Interrupt, Icebreak, and Routine, with situational awareness capability.

-   :material-file-document-multiple:{ .lg .middle } __Rule Management__

    ---

    Rules stored as Markdown files with hot reload support. Multi-level scope (global > Bot > group). AI may manage rules autonomously via ToolSet.

-   :material-clock-outline:{ .lg .middle } __Cron System__

    ---

    Efficient time-wheel-based task scheduling. Supports one-shot and recurring reminders defined by Cron expressions. AI generates natural reminder text.

-   :material-text-box-search:{ .lg .middle } __Summary System__

    ---

    Automatic generation of group chat conversation summaries. AI extracts key information and topic threads, with historical summary retrieval.

-   :material-dna:{ .lg .middle } __Evolution System__

    ---

    Automatic learning of new vocabulary and trending expressions from group chats. Records semantic meaning and usage context, integrating them naturally at appropriate moments.

</div>

## Plugin Extensions

erii is built on the PF4J plugin framework and supports four extension points:

| Extension Type | Description | Trigger |
|:---|:---|:---|
| **AgentExtension** | General agent extension | Always participates in agent loop |
| **RouteExtension** | LLM routing extension | Matched by RoutingAgent intent classification |
| **CmdExtension** | Command extension | `/xxx` command prefix match, supports aliases |
| **PassiveExtension** | Passive extension | Background tasks / event listeners |

## Component Services

| Component | Description | Supported Backends |
|:---|:---|:---|
| **LLM Chat** | AI conversational reasoning | Google Gemini / DeepSeek / MiniMax / OpenAI / Anthropic / OpenRouter |
| **Vector Embedding** | Text and image semantic vectorization | ByteDance Embedding / SiliconFlow |
| **Web Search** | Real-time search for AI Agent | Exa / MiniMax Search |
| **Vision** | Image content understanding and description | MiniMax Vision |
| **Browser** | Web page screenshots and Markdown scraping | Playwright |

## Tech Stack

<div class="grid cards" markdown>

-   __Backend__

    ---

    Kotlin 2.2+ &middot; Ktor 3.3+ &middot; Koin 4.1+ &middot; JobRunr 8.3+ &middot; H2 &middot; Exposed &middot; PF4J 3.15+

-   __AI Framework__

    ---

    Koog Agents 0.7+ &middot; Multi-provider LLM support

-   __CLI Tool__

    ---

    Go 1.21+ &middot; Bubble Tea TUI

-   __Distribution__

    ---

    npm monorepo &middot; Node.js CLI launcher

</div>
