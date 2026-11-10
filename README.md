<p align="center">
  <img src="assets/logo.png" alt="Trenz" width="96" height="96">
</p>

<h1 align="center">Trenz MCP</h1>

<p align="center">
  Research TikTok Shop products, generate AI content and manage your social accounts from Claude, ChatGPT, Cursor, Codex and Gemini CLI.
</p>

<p align="center">
  <a href="https://mcp.trenz.ai/?utm_source=github">Documentation</a> ·
  <a href="https://www.trenz.ai/?utm_source=github">trenz.ai</a> ·
  <a href="https://mcp.trenz.ai/tools?utm_source=github">Tools reference</a>
</p>

Trenz MCP connects your [trenz.ai](https://www.trenz.ai/?utm_source=github) account to the AI assistant you already use. After you connect, ask the assistant to research TikTok Shop products, generate videos and images, or check your social accounts, and it uses Trenz to do the work.

This repository contains the connection settings, the Claude Code and Cursor plugin manifests, the Gemini CLI extension manifest, the Trenz skill, and the [`server.json`](server.json) published to the [official MCP Registry](https://registry.modelcontextprotocol.io/v0/servers?search=ai.trenz) as `ai.trenz/mcp`. The Trenz MCP server itself is hosted by Trenz; no server code or credentials are stored here.

## Server URL

```
https://mcp.trenz.ai/mcp/oauth
```

Remote MCP over streamable HTTP. You sign in with your trenz.ai account the first time you connect; there is no API key.

## What you can do

- **Product research**: search products, creators, videos, shops, ads and livestreams, open full details, and read rankings.
- **AI content**: generate product videos, images and scripts, analyze videos, and build AI canvases.
- **Social accounts**: check performance, publish posts, reply to comments and messages, and read your TikTok Shop seller reports.

## Connect

### Claude Code

Install the plugin. It adds the Trenz MCP server and the Trenz skill:

```
/plugin marketplace add trenzai/trenz-mcp
/plugin install trenz@trenz
```

Or add only the MCP server:

```bash
claude mcp add --transport http trenz https://mcp.trenz.ai/mcp/oauth
```

Then run `/mcp`, select **trenz**, choose **Authenticate**, sign in to trenz.ai if asked and click **Allow**.

### Cursor

[![Add Trenz to Cursor](https://cursor.com/deeplink/mcp-install-dark.svg)](https://cursor.com/en/install-mcp?name=trenz&config=eyJ1cmwiOiJodHRwczovL21jcC50cmVuei5haS9tY3Avb2F1dGg/c3JjPWdpdGh1YiJ9)

Or add this to `~/.cursor/mcp.json` (merge it with any servers already there):

```json
{
  "mcpServers": {
    "trenz": {
      "url": "https://mcp.trenz.ai/mcp/oauth"
    }
  }
}
```

Then open **Cursor Settings** → **MCP**, click **Login** next to trenz, sign in to trenz.ai if asked and click **Allow**.

### Codex

```bash
codex mcp add trenz --url https://mcp.trenz.ai/mcp/oauth
```

A trenz.ai page opens; sign in if asked and click **Allow**. If the browser doesn't open, run `codex mcp login trenz`.

### Gemini CLI

```bash
gemini extensions install https://github.com/trenzai/trenz-mcp
```

The first time Gemini uses Trenz, a trenz.ai page opens; sign in if asked and click **Allow**. If it doesn't open, run `/mcp auth trenz` in Gemini CLI.

### Claude (web, desktop, mobile)

**Settings** → **Connectors** → **Add custom connector**. Name `Trenz`, URL `https://mcp.trenz.ai/mcp/oauth`, click **Add**, then **Connect** and **Allow** on trenz.ai. On Claude Team and Enterprise, an Owner adds the connector first in **Organization settings** → **Connectors**.

### ChatGPT

On chatgpt.com in a browser: [chatgpt.com/plugins](https://chatgpt.com/plugins) → **+** → **Add custom MCP server**. Name `Trenz`, URL `https://mcp.trenz.ai/mcp/oauth`, authentication **OAuth**, then **Create as a plugin** and **Allow** on trenz.ai. Which ChatGPT plans can add custom MCP servers is decided by OpenAI; see [Connect ChatGPT](https://mcp.trenz.ai/connect/chatgpt?utm_source=github).

### Other assistants

Assistants that support remote MCP servers with OAuth can use the same URL.

Step-by-step guides with screenshots: [mcp.trenz.ai](https://mcp.trenz.ai/?utm_source=github).

## Trenz skill

[`skills/trenz`](skills/trenz) gives the assistant step-by-step workflows for product selection, competitor and creator analysis, profit estimates, content generation and social media work. The Claude Code plugin installs it for you. For Codex or a manual install, copy `skills/trenz` into `~/.codex/skills/` or `~/.claude/skills/`, or download the [skill package](https://mcp.trenz.ai/skill?utm_source=github).

## Check that it works

After connecting, send this to your assistant:

```
Show my Trenz credits balance and list what you can do with Trenz.
```

## Plans and billing

Trenz MCP is included in every trenz.ai plan, including Free. It uses the same account, permissions and credits as the website.

- Data queries do not use credits.
- AI generation uses credits, at the same price as on the website.
- Each plan includes a number of assistant calls every 7 days.

Details: [Usage limits and credits](https://mcp.trenz.ai/usage-limits-and-credits?utm_source=github).

## License

MIT. See [LICENSE](LICENSE).
