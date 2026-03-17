# sub2api-opencode-skill

Agent skill for configuring [Sub2API](https://github.com/Wei-Shaw/sub2api) as a provider in [OpenCode](https://opencode.ai).

## What it does

Helps you correctly set up Sub2API's Antigravity proxy as an OpenCode provider, avoiding the common 404 pitfalls caused by SDK/endpoint mismatches.

**Key insight:** Sub2API's `/antigravity` endpoint uses Anthropic Messages API (`/v1/messages`) for Claude and Google Generative AI API (`/v1beta/`) for Gemini -- **not** OpenAI-compatible `/v1/chat/completions`.

## Install

```bash
npx skills add likai/sub2api-opencode-skill
```

## What's inside

- Correct SDK mapping for each model family
- Ready-to-use `opencode.json` templates (Claude-only, Gemini-only, Full setup)
- Step-by-step setup with connectivity verification
- Comprehensive troubleshooting for 404, 503, config errors
- Nginx reverse proxy notes
- Environment variable security best practices

## Quick Config (Claude via Antigravity)

```json
{
  "$schema": "https://opencode.ai/config.json",
  "model": "sub2api-claude/claude-sonnet-4-6",
  "provider": {
    "sub2api-claude": {
      "npm": "@ai-sdk/anthropic",
      "name": "Sub2API Claude",
      "options": {
        "baseURL": "https://your-sub2api-domain/antigravity/v1",
        "apiKey": "sk-your-api-key"
      },
      "models": {
        "claude-sonnet-4-6": {
          "name": "Claude Sonnet 4.6",
          "limit": { "context": 200000, "output": 64000 },
          "modalities": {
            "input": ["text", "image", "pdf"],
            "output": ["text"]
          }
        }
      }
    }
  }
}
```

## Common Mistakes

| Mistake | Result | Fix |
|---|---|---|
| Using `@ai-sdk/openai-compatible` for Claude | 404 | Use `@ai-sdk/anthropic` |
| Using `@ai-sdk/openai-compatible` for Gemini | 404 | Use `@ai-sdk/google-generative-ai` |
| Using `/v1` baseURL for Gemini | 404 | Use `/v1beta` |
| Missing `modalities` in model config | Images/PDF rejected | Add `"input": ["text", "image", "pdf"]` |
| `provider` as string instead of object | Config invalid | Use `"provider": { ... }` |

## References

- [Sub2API](https://github.com/Wei-Shaw/sub2api) - AI API Gateway (6K+ stars)
- [OpenCode](https://opencode.ai) - Terminal AI coding assistant
- [OpenCode Provider Docs](https://opencode.ai/docs/providers/)

## License

MIT
