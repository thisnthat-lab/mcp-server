# this+that MCP Server Plugins

Official MCP plugins for the [this+that](https://thisandthat.chat) productivity platform. Designed for Claude Code, but the underlying MCP servers work with any client that speaks the [Model Context Protocol](https://modelcontextprotocol.io/).

## Available plugins

- [`dobox-cowork`](./dobox-cowork) — view your DoBox tasks in your AI assistant via the `/dobox` command.

## Install in Claude Code

```
/plugin marketplace add thisnthat-lab/mcp-server
/plugin install dobox-cowork@thisnthat
```

For per-plugin documentation, see each plugin's README.

## Use the MCP server from any other client

The MCP servers powering these plugins are public HTTPS endpoints. Any compliant MCP client (Claude Desktop, Cursor, custom integrations, etc.) can connect directly without installing this plugin. See each plugin's README for the server URL and the tools it exposes.

## License

MIT
