# Environment Variables

`.env.local` is located in the project `conf/` directory, storing API keys and sensitive information.

## LLM API Keys

| Variable | Provider |
|:---|:---|
| `GOOGLE_API_KEY` | Google Gemini |
| `OPENAI_API_KEY` | OpenAI |
| `ANTHROPIC_API_KEY` | Anthropic |
| `OPENROUTER_API_KEY` | OpenRouter |

## Service Components

| Variable | Description |
|:---|:---|
| `EMBEDDING_API_KEY` | Vector embedding API key |
| `SEARCH_API_KEY` | Web search API key |
| `VISION_API_KEY` | Vision analysis API key |

## Server Configuration

| Variable | Default | Description |
|:---|:---|:---|
| `ERII_SERVER_USERNAME` | `eriix` | Admin username |
| `ERII_SERVER_PASSWORD` | `@Aa123!` | Admin password |
| `ERII_PORT` | `8080` | Server port |
| `ERII_START_MODE` | — | Set to `CLI` for file-only logging |
| `ERII_JOBRUNR_DASHBOARD` | `false` | Enable job dashboard |
| `ERII_JOBRUNR_DASHBOARD_PORT` | `8000` | Dashboard port |
| `ERII_H2_CONSOLE` | `false` | Enable database console |
| `ERII_H2_CONSOLE_PORT` | `8082` | Console port |
| `ERII_MIGRATION` | `false` | Enable DB migration |

## Proxy

| Variable | Description |
|:---|:---|
| `HTTP_PROXY` | HTTP proxy address |
| `SOCKS_PROXY` | SOCKS proxy address |
