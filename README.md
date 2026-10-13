# Synento for Cursor

[Synento](https://synento.com) is agent-native video: live video calls that
are also their own recordings, built so a coding agent can set up and verify
the whole integration without anyone clicking through a dashboard.

This plugin adds:

- **The Synento MCP server** (`https://api.synento.com/mcp`). The agent can
  create projects and sessions, mint restricted server keys, and run
  `verify_session`, which reads a call's recording back to confirm camera and
  microphone media actually arrived.
- **A `synento-video` skill** that walks the agent through the integration
  and tells it not to call the job done until `verify_session` passes.

## Sign in

The first time the agent uses the server, Cursor opens your browser so you
can sign in to Synento and choose which projects the agent may use. You can
see its recent tool calls and disconnect it from the dashboard under
**Agents**.

## Without the plugin

You can also add the server by hand to `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "synento": { "url": "https://api.synento.com/mcp" }
  }
}
```

## Links

- [MCP guide](https://synento.com/docs/guides/mcp)
- [Documentation](https://synento.com/docs)
- Support: hello@synento.com
