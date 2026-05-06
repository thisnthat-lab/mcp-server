# DoBox for AI assistants

Your DoBox, in any AI assistant that speaks MCP.

This is the official this+that DoBox connector. It lets your AI assistant — Claude Code, Claude Desktop, Cursor, or any MCP-compatible client — read your DoBox entries on demand: overdue items, today's work, upcoming tasks, AI suggestions. Ask questions like "what's overdue?" or "summarize my DoBox for the day" without leaving your editor or terminal.

The connector is read-only and authenticated against your existing this+that account. There are no API keys to manage; sign-in happens once in your browser via OAuth.

---

## What you'll need

- An active **this+that account**. If you don't have one, sign up at [assistant.thisandthat.chat](https://assistant.thisandthat.chat) first and add at least one DoBox entry.
- An **MCP-compatible AI client**. Many users will use Claude Code or another AI Chat program, but the connector works with any client that supports the MCP standard.

That's it.

---

## Install in Claude Code

The fastest path is one terminal command:

```
claude mcp add --transport http this_and_that https://mcp.thisandthat.chat/mcp
```

Then in Claude Code, run `/mcp` to verify the connection. Your browser opens for sign-in the first time; Claude remembers you on that machine afterwards.

(There's also a plugin-install path via `/plugin marketplace add thisnthat-lab/mcp-server` followed by `/plugin install dobox-cowork@thisnthat`. It works, but Claude doesn't expose MCP tools as slash commands so it gives you no shortcut over the direct add — see "Asking your assistant" below.)

## Asking your assistant

Once the connector is set up, ask in plain English:

- "What's in my DoBox?"
- "What's overdue?"
- "Summarize my unresolved DoBox entries"
- "Show me 10 resolved DoBox items"

Your assistant picks the right tool and arguments automatically. **There is no `/dobox` or `/get_dobox_entries` slash command in Claude** (Code, Desktop, or web) — Claude doesn't expose MCP tools as slash commands. Use natural language.

> **ChatGPT exception**: ChatGPT does auto-generate a `/dobox` slash shortcut after you add the connector, so you can type `/dobox` directly. ChatGPT is the only client we've seen do this.

---

## Use it from any other MCP client

The DoBox MCP server is a public, standard MCP HTTP endpoint. Any compliant client can connect to it directly — you don't need our Cowork plugin.

**Server URL:** `https://mcp.thisandthat.chat/mcp`

**Auth:** OAuth 2.0 with Dynamic Client Registration. Your client registers itself automatically the first time it connects; you sign in via your normal this+that browser session.

### Claude Desktop

Add this to your Claude Desktop config (Settings → Developer → Edit Config):

```json
{
  "mcpServers": {
    "dobox": {
      "type": "http",
      "url": "https://mcp.thisandthat.chat/mcp"
    }
  }
}
```

Restart Claude Desktop. On first use, it'll prompt you to sign in.

### ChatGPT (custom MCP connector)

Requires **Developer Mode** enabled. On the web app: **Settings → Apps → Advanced settings → Developer mode** (Pro/Plus/Business/Enterprise/Education accounts; web only — not mobile).

Then **Settings → Connectors → Create custom connector** with:

- **Server URL**: `https://mcp.thisandthat.chat/mcp`
- Leave all other fields (Client ID, Client Secret, advanced URLs) **blank** — the server handles dynamic client registration

Save and click Connect. You'll be redirected to sign in. After authorizing, the connector activates and DoBox tools appear in your conversations.

### Other MCP clients

Point your client at `https://mcp.thisandthat.chat/mcp` as an HTTP MCP server. The OAuth metadata endpoints are:

- `https://mcp.thisandthat.chat/.well-known/oauth-protected-resource`
- `https://mcp.thisandthat.chat/.well-known/oauth-authorization-server`

Most clients discover these automatically.

### Tested clients

| Client | Connection | Tool calls | Notes |
|---|---|---|---|
| Claude Code (via this plugin) | Untested | Untested | Recommended path; pending end-to-end verification |
| claude.ai web (custom connector) | Working | Working | Verified end-to-end |
| Claude Desktop (custom connector) | Working | Working | Same broker as claude.ai web |
| ChatGPT (custom connector) | Working | Working | Verified end-to-end. Requires Developer Mode. |
| MCP Inspector 0.21.2 | Working | Working | Verified end-to-end |
| Gemini | Untested | Untested | Should support remote MCP via Vertex/Agentspace — reports welcome via [issues](https://github.com/thisnthat-lab/mcp-server/issues). |
| Cursor, Cline, Continue, Zed, others | Unknown | Unknown | If you're using one of these, we'd love a report. |

---

## Tools available

| Tool | What it does |
|---|---|
| `dobox_get_entries` | Fetch your DoBox entries. Accepts a `filter` (`UNRESOLVED` or `RESOLVED`, default `UNRESOLVED`) and `limit` (1–50, default 20). |

More tools (creating, updating, and resolving tasks) are on the roadmap.

---

## What data does it access?

The connector reads entries from your DoBox — subject, description, due date, attached communications, and AI-suggested status. It cannot read other users' data, and it does not currently write or delete anything.

Authentication uses standard OAuth 2.0. Tokens are stored by your AI client (e.g., in your OS keychain on macOS); we never see them.

For full details on how this+that handles your data, see the [privacy policy](https://www.thisandthat.chat/assistant-privacy-policy).

---

## Troubleshooting

**"The DoBox connector isn't connected" or the `/dobox` command does nothing.**
The plugin's MCP server hasn't completed OAuth yet. In Claude Code, run `/plugin` and confirm `dobox-cowork` is enabled. Then run `/dobox` again — your browser should open for sign-in.

**Browser opens but sign-in fails.**
Make sure you're signed in to your this+that account at [thisandthat.chat](https://thisandthat.chat) in the same browser. If you have multiple this+that accounts, sign out and back in with the right one before retrying.

**The plugin says "no entries" but you definitely have some.**
Check the `filter` — by default the tool returns unresolved entries only. If everything's resolved, that explains an empty list. You can ask Claude to fetch resolved entries explicitly.

**Authenticated but tool calls fail with 401.**
Your OAuth token may have expired. Reconnect by removing and re-installing the plugin (Claude Code: `/plugin uninstall dobox-cowork@thisnthat` then re-install), or by clearing the saved credentials in your client's config.

**Something else?**
Email support@thisandthat.chat with the error you're seeing.

---

## Uninstall

In Claude Code:

```
/plugin uninstall dobox-cowork@thisnthat
```

Uninstalling removes the plugin from your AI client and clears its stored credentials. If you want to be extra-thorough, sign out of this+that on [thisandthat.chat](https://thisandthat.chat) and back in, which invalidates the OAuth session at the source.
