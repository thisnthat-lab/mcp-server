# DoBox for Claude

Your DoBox, in any AI assistant that speaks MCP.

This is the official this+that DoBox connector. It lets Claude (or any MCP-compatible AI client) read your DoBox entries on demand — overdue items, today's work, upcoming tasks, AI suggestions — so you can ask questions like "what's overdue?" or "summarize my DoBox for the day" without leaving your editor or terminal.

The connector is read-only and authenticated against your existing this+that account. There are no API keys to manage; sign-in happens once in your browser via OAuth.

---

## What you'll need

- An active **this+that account**. If you don't have one, sign up at [thisandthat.chat](https://thisandthat.chat) first and add at least one DoBox entry.
- An **MCP-compatible AI client**. Most users will use Claude Code, but the connector works with any client that supports the MCP standard (Claude Desktop, etc.).

That's it.

---

## Install in Claude Code (recommended)

Claude Code is the easiest path because we ship a Cowork plugin that bundles the connector with a `/dobox` slash command.

In Claude Code, run:

```
/plugin marketplace add thisnthat-lab/mcp-server
/plugin install dobox-cowork@thisnthat
```

The first time you run a DoBox command, Claude Code will open your browser to sign in to this+that. After that, it remembers you on that machine.

### Try it

In any Claude Code conversation, type:

```
/dobox
```

Claude will fetch your unresolved DoBox entries and group them by due date (overdue, today, upcoming, no due date).

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

### Other MCP clients (Cursor, custom integrations, etc.)

Point your client at `https://mcp.thisandthat.chat/mcp` as an HTTP MCP server. The OAuth metadata endpoints are:

- `https://mcp.thisandthat.chat/.well-known/oauth-protected-resource`
- `https://mcp.thisandthat.chat/.well-known/oauth-authorization-server`

Most clients discover these automatically.

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
