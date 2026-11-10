# Connecting Trenz

Use this guide when the Trenz MCP tools are unavailable.

Keep the user's original task paused while guiding: don't complete it with other data sources in the meantime. Return to it after the connection is verified.

## Contents

- [How Trenz connects](#how-trenz-connects)
- [Add the server](#add-the-server)
- [Verify](#verify-yourself-first-then-ask-the-user)
- [Troubleshooting](#troubleshooting-identify-the-kind-of-failure-first)

## How Trenz connects

- **Server name**: `trenz` (lowercase)
- **URL**: `https://mcp.trenz.ai/mcp/oauth` (streamable HTTP), including `/mcp/oauth`
- **Authentication**: OAuth only. The client opens a trenz.ai page; the user signs in and clicks **Allow**. Trenz has no API keys: don't ask for one, and leave any API key or header fields empty.
- The connection runs on the trenz.ai account the user signs in with, including that account's plan, 7-day Agent allowance and credits.

Never ask for an OAuth code, access token or refresh token, and never sign in or click **Allow** for the user.

Setup steps for every client are at https://mcp.trenz.ai (Connect section). Identify the current client first, then follow the matching section below.

## Add the server

### Claude Code

You can run the command yourself:

```bash
claude mcp add --transport http trenz https://mcp.trenz.ai/mcp/oauth
```

Add `--scope user` to make Trenz available in every project. Then ask the user to run `/mcp`, select **trenz**, choose **Authenticate**, sign in to trenz.ai if asked and click **Allow**. A new session loads the tools.

### Codex (CLI, app and IDE extension)

You can run the command yourself:

```bash
codex mcp add trenz --url https://mcp.trenz.ai/mcp/oauth
```

Codex opens a trenz.ai page; the user signs in if asked and clicks **Allow**. If the browser doesn't open, run `codex mcp login trenz`. The CLI, app and IDE extension share this configuration. Start a new session so the tools load.

If the user prefers the Codex app: **Plugins** in the sidebar → settings icon (**Manage**) → **MCPs** tab → **Add** → **Add MCP server**; name `trenz`, type **Streamable HTTP**, the URL above; **Save**, then **Authenticate** next to trenz and **Allow** on trenz.ai.

### Cursor

Add the entry to `~/.cursor/mcp.json` (all projects) or `.cursor/mcp.json` (one project). If the file already has other servers, merge; don't overwrite:

```json
{
  "mcpServers": {
    "trenz": {
      "url": "https://mcp.trenz.ai/mcp/oauth"
    }
  }
}
```

Then the user opens **Cursor Settings** → **MCP**, clicks **Login** (or **Connect**) next to trenz, signs in if asked and clicks **Allow**.

### Claude (web, desktop, mobile)

The user does this in the Claude app: **Settings** → **Connectors** → **Add custom connector**, name `Trenz`, URL above, **Add**, then **Connect** and **Allow** on trenz.ai. On Claude Team and Enterprise, an Owner adds the connector first in **Organization settings** → **Connectors**.

### ChatGPT

The user does this on chatgpt.com in a browser: [chatgpt.com/plugins](https://chatgpt.com/plugins) → **+** → **Add custom MCP server**, name `Trenz`, URL above, authentication **OAuth**, confirm the warning, **Create as a plugin**, then **Allow** on trenz.ai. Which ChatGPT plans can add custom MCP servers is decided by OpenAI; see https://mcp.trenz.ai/connect/chatgpt.

### Other clients

Use the client's own documentation for remote MCP (streamable HTTP) with OAuth and the URL above. Don't write a configuration format from memory. If the client can't do OAuth, it can't connect to Trenz.

Opening the trenz.ai page is not the same as being connected. Ask the user to reply once they've clicked **Allow**, then verify.

## Verify (yourself first, then ask the user)

1. **Verify yourself.** Two things to watch for, so you don't conclude "not configured" by mistake:
   - Tool names usually carry the server prefix (for example `mcp__trenz__account_info`), not the bare `account_info`.
   - Tools may load lazily: if the client has a tool search or loading mechanism, search for trenz and load the schema before calling.

   Call `account_info` by its full name:
   - It returns account information → connected. Return to the original task; the user doesn't need to do anything else.
   - 401 / authentication error → go to "Authorization expired".
   - A prefix search finds **no** trenz tools at all → go to step 2.
2. **Ask the user to check** (only once the tools really aren't there). The actions are the same in every client: check the configuration, enable or trust the server, reload the connection.
   - Configuration not picked up → check the file syntax (JSON/TOML commas and quotes, `disabled` set to true) and that you edited the file the client actually reads.
   - Not enabled or not trusted → turn it on; accept prompts such as "Trust this server".
   - Enabled but no tools → reload: start a new session in Claude Code or Codex, reconnect in the client's MCP or connector settings elsewhere.

   Then verify again from step 1. Don't ask the user to repeat the same action; after two failed rounds, stop and show the user the exact error.

## Troubleshooting (identify the kind of failure first)

### Connection lost (worked before; now connection errors, timeouts, or the tools disappeared)

Not an authorization problem. Reload the MCP connection (new session in Claude Code or Codex; reconnect in the client's settings elsewhere), then verify with `account_info`. If it still fails, check the network or whether the configuration was changed.

### Authorization expired (401, authentication error, or "Your trenz.ai sign-in has expired")

Ask the user to sign in to trenz.ai, then reconnect Trenz in the client and click **Allow** again (Claude Code: `/mcp` → **Authenticate**; Codex CLI: `codex mcp login trenz`; Codex app: **Authenticate** in the **MCPs** list). Verify with `account_info`.

### Wrong trenz.ai account

The user signs out of trenz.ai in the browser, signs in with the right account, then removes and adds Trenz again in the client.

### Plan, allowance or credit messages

Messages such as reaching the usage limit of the current plan, a capability not included in the plan, or a credit balance too low for a generation task are not connection failures. Explain them with the guidance in the response and continue with what the plan allows; don't reconnect.

Don't keep retrying a failing tool call. After two failures, stop and follow the matching section above.
