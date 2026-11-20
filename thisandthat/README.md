# this+that for AI assistants

Your DoBox, your Collections and Company Brain, and your own memory, in any AI assistant that speaks MCP.

This is the official this+that connector. It lets your AI assistant (Claude Code, Claude Desktop, Cursor or any MCP-compatible client) work with three parts of this+that:

- **DoBox**: read, create, update and resolve tasks, with comments, checklists and subtasks; accept suggested tasks; organize them into groups, subgroups and labels; set up rules and recurring tasks
- **Collections and the Company Brain**: search and read your team's Collections pages and, on the Enterprise plan, its Company Brain pages; create, edit and rename Collections pages and spaces (the Company Brain writes its own pages, so they're read-only)
- **Your memory**: what your own mail, chats and notes established (commitments, decisions, deadlines, preferences), who you deal with most, your profile and writing voice; you can confirm, correct or retract what it holds

Ask things like "what's overdue?", "what did I promise Halcyon?" or "add the new payment terms to the Halcyon page" without leaving your editor or terminal.

The connector is authenticated against your existing this+that account. There are no API keys to manage; sign-in happens once in your browser via OAuth.

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

To get the slash commands below as well, install the plugin instead:

```
/plugin marketplace add thisnthat-lab/mcp-server
/plugin install thisandthat@thisnthat
```

The plugin connects the same server and adds the commands.

## Asking your assistant

Once the connector is set up, ask in plain English:

- "What's in my DoBox?"
- "What's overdue?"
- "Create a task to send Halcyon the renewal quote by Friday"
- "What do our Collections say about the refund policy?"
- "What have I committed to this week?"

Your assistant picks the right tools and arguments.

### Slash commands (with the plugin)

Installing the plugin in Claude Code adds five commands for the things people do most:

| Command | What it does |
|---|---|
| `/ask <question>` | Searches your Collections, Company Brain and own memory, and answers with sources |
| `/person <name or email>` | Briefs you before a conversation: your history with them, open commitments both ways, and open tasks that involve them |
| `/commitments` | What you've promised and what you're owed, overdue first |
| `/task <what, by when>` | Turns a sentence into a DoBox task, after you confirm it |
| `/dobox` | Lists your open DoBox tasks, grouped by due date |

Connecting the server directly, without the plugin, gives you every tool but none of these commands: Claude doesn't turn MCP tools into slash commands on its own.

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
    "thisandthat": {
      "type": "http",
      "url": "https://mcp.thisandthat.chat/mcp"
    }
  }
}
```

Restart Claude Desktop. On first use, it'll prompt you to sign in.

### ChatGPT (custom MCP server)

On chatgpt.com (web only):

1. Go to [chatgpt.com/plugins](https://chatgpt.com/plugins), select the plus button, then **Add custom MCP server**
2. Name it, and enter `https://mcp.thisandthat.chat/mcp` as the **Server URL**
3. Choose **OAuth**, leave every other field blank (the server handles client registration), confirm, and select **Create as a plugin**
4. Install the plugin and sign in when prompted
5. In a new chat, type `@` and pick the plugin

Your workspace's settings decide whether you can add custom MCP servers, so on a Business or Enterprise plan an admin may need to allow it. Writes ask for your confirmation first. Deep research and company knowledge use the `search` and `fetch` tools to find and cite your Collections and Company Brain pages.

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
| ChatGPT (custom MCP server) | Working | Working | Verified end-to-end |
| MCP Inspector 0.21.2 | Working | Working | Verified end-to-end |
| Gemini | Untested | Untested | Should support remote MCP via Vertex/Agentspace — reports welcome via [issues](https://github.com/thisnthat-lab/mcp-server/issues). |
| Cursor, Cline, Continue, Zed, others | Unknown | Unknown | If you're using one of these, we'd love a report. |

---

## Tools available

The server exposes 64 tools. Your client lists each one with its full description; these are the groups.

| Group | Tools | What they do |
|---|---|---|
| DoBox tasks | `dobox_get_entries`, `dobox_list`, `dobox_get`, `create_dobox`, `update_dobox`, `resolve_dobox`, `accept_dobox_suggestion`, `update_dobox_collections`, `set_dobox_parent`, `move_dobox_to_subgroup` | List, read, create, update and resolve tasks; accept a suggested task; file tasks into groups and under parent tasks |
| Task detail | `add_dobox_comment`, `list_dobox_comments`, `get_dobox_comment_upload_url`, `read_dobox_comment_attachment`, `add_dobox_checklist`, `add_dobox_subtask`, `toggle_dobox_subtask` | Comments and their attachments, checklists and subtasks |
| Organizing | `list_task_groups`, `create_task_group`, `update_task_group`, `update_task_group_participants`, `list_task_subgroups`, `get_task_subgroup`, `create_task_subgroup`, `update_task_subgroup`, `list_task_labels`, `create_task_label`, `update_task_label` | Task groups, who's in them, subgroups and labels |
| Automation | `list_dobox_rules`, `create_dobox_rule`, `update_dobox_rule`, `list_recurring_dobox`, `get_recurring_dobox_by_reference`, `create_recurring_dobox`, `update_recurring_dobox` | Rules that act on incoming tasks, and recurring tasks |
| Teams | `list_user_teams`, `get_team`, `list_team_members` | Your teams and their members |
| Collections and Company Brain | `knowledge_search`, `knowledge_get_page`, `knowledge_list_pages`, `knowledge_list_spaces`, `knowledge_create_page`, `knowledge_edit_page`, `knowledge_rename_page`, `knowledge_create_space`, `knowledge_rename_space` | Search and read Collections and Company Brain pages; write Collections pages |
| ChatGPT search | `search`, `fetch` | Search your Collections and Company Brain pages and read one in full, in the shape ChatGPT deep research and company knowledge expect; results link back to the page in this+that |
| Your memory | `user_brain_status`, `user_brain_profile`, `user_brain_list_memories`, `user_brain_search_memories`, `user_brain_top_contacts`, `user_brain_person_context`, `user_brain_hierarchy`, `user_brain_voice`, `user_brain_signals`, `user_brain_review_queue`, `user_brain_remember`, `user_brain_declare_fact`, `user_brain_confirm_memory`, `user_brain_retract_memory`, `user_brain_forget_person` | Read what your mail and chats established, add or confirm facts, and retract a memory or forget a person |

Every tool is marked read-only or write in its MCP annotations, so clients that ask before a write will ask.

---

## What data does it access?

The connector acts as you, with your this+that permissions. It reads and changes your DoBox tasks and their comments, groups, rules and recurring tasks; reads the Collections and Company Brain pages your team shares with you and edits the Collections ones; and reads and updates your own memory. It sees only what your account can already see in this+that. Two tools are marked destructive: `user_brain_retract_memory` drops a memory you say is wrong, and `user_brain_forget_person` erases what your memory holds about one person and stops tracking them, which can't be undone.

Authentication uses standard OAuth 2.0. Our server issues the tokens and your AI client stores them (on macOS, usually in your keychain).

For full details on how this+that handles your data, see the [privacy policy](https://www.thisandthat.chat/assistant-privacy-policy).

---

## Troubleshooting

**"The DoBox connector isn't connected" or the `/dobox` command does nothing.**
The plugin's MCP server hasn't completed OAuth yet. In Claude Code, run `/plugin` and confirm `thisandthat` is enabled. Then run `/dobox` again — your browser should open for sign-in.

**Browser opens but sign-in fails.**
Make sure you're signed in to your this+that account at [thisandthat.chat](https://thisandthat.chat) in the same browser. If you have multiple this+that accounts, sign out and back in with the right one before retrying.

**The plugin says "no entries" but you definitely have some.**
Check the `filter` — by default the tool returns unresolved entries only. If everything's resolved, that explains an empty list. You can ask Claude to fetch resolved entries explicitly.

**Authenticated but tool calls fail with 401.**
Your OAuth token may have expired. Reconnect by removing and re-installing the plugin (Claude Code: `/plugin uninstall thisandthat@thisnthat` then re-install), or by clearing the saved credentials in your client's config.

**Something else?**
Email support@thisandthat.chat with the error you're seeing.

---

## Uninstall

In Claude Code:

```
/plugin uninstall thisandthat@thisnthat
```

Uninstalling removes the plugin from your AI client and clears its stored credentials. If you want to be extra-thorough, sign out of this+that on [thisandthat.chat](https://thisandthat.chat) and back in, which invalidates the OAuth session at the source.
