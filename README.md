# HyperFrames MCP server

Hosted [Model Context Protocol](https://modelcontextprotocol.io/) server for **HyperFrames** — generate images and video straight from Claude, Cursor, VS Code or any other MCP client. No install, no local process: you connect to a URL and authenticate with an API key.

> **Name note:** this is the hosted service at [hyperframes.net](https://hyperframes.net). It is not HeyGen's HyperFrames Studio (hyperframes.studio) and not the open-source `hyperframes` renderer. [What each one does](https://hyperframes.net/blog/is-hyperframes-free).

- Endpoint: `https://hyperframes.net/api/mcp`
- Transport: Streamable HTTP
- Auth: `Authorization: Bearer sk-hh1_...`
- Docs: <https://hyperframes.net/hyperframes-mcp>

## Setup

1. Create an account at <https://hyperframes.net/sign-up>. A new account starts with 20 credits.
2. Create an API key under <https://hyperframes.net/settings/apikeys>. The secret is shown once.
3. Point your client at the endpoint.

**Claude Code**

```bash
claude mcp add --transport http hyperframes https://hyperframes.net/api/mcp \
  --header "Authorization: Bearer sk-hh1_YOUR_KEY"
```

**`mcp.json`** (Cursor, VS Code, and other clients that read this format)

```json
{
  "mcpServers": {
    "hyperframes": {
      "type": "http",
      "url": "https://hyperframes.net/api/mcp",
      "headers": {
        "Authorization": "Bearer sk-hh1_YOUR_KEY"
      }
    }
  }
}
```

## Tools

| Tool | What it does |
| --- | --- |
| `list_models` | Video and image scenes this account can generate, with the credit cost of each. |
| `get_credits` | Current balance, split into promotional and purchased credits. |
| `generate_image` | Text-to-image, or edit an existing image when `image_url` is supplied. |
| `generate_video` | Text-to-video, image-to-video or reference-to-video. |
| `get_generation` | Poll one job by id until it reaches `success`, `failed` or `canceled`. |

Generation is asynchronous: `generate_image` / `generate_video` return a task id, and you poll `get_generation` for the result.

## Credits

Promotional credits (the signup bonus, gifts) cover image generation. Video generation is charged against purchased credits only, because it carries a real per-second provider cost. `list_models` and `get_credits` report the exact prices for the calling account.

## Errors

Tool failures come back as results with `isError: true` and a human-readable message — for example `insufficient credits`, an unknown scene name, or a prompt over 4000 characters. A missing or invalid API key is rejected at the HTTP layer with `401`.

## Links

- Product page: <https://hyperframes.net/hyperframes-mcp>
- Pricing and credits: <https://hyperframes.net/pricing>
- Registry entry: `net.hyperframes/hyperframes` in the [official MCP Registry](https://registry.modelcontextprotocol.io)
