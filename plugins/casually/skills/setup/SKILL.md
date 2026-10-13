---
name: setup
description: Connect Codex or Claude to Casually with browser-based MCP OAuth, including staging and local hosts, then verify the connection with read-only tools.
---

# Connect Casually

For Claude Code desktop installation, guide the user through Manage Plugins → Marketplaces → paste `https://github.com/clyr-inc/casually-plugins.git` → Add → Plugins → Casually → Install. Open a new conversation after installation. CLI installation is an optional alternative.

Use the client’s native HTTP MCP OAuth login. Do not issue, request, copy, or store an API key. Never ask for a password, access token, refresh token, or authentication callback URL in chat.

1. Ask which Casually environment the user intends to use if it is unknown. The bundled MCP server uses `https://api.casually.marketing/api/v2/mcp` for production.
2. Claude Code: open `/mcp`, select the plugin’s Casually server, and choose Authenticate. Codex: open the MCP settings and choose login for the Casually server.
3. The user logs in to Casually in the browser, confirms the account, API host, callback destination and requested scopes, and selects “接続を許可”. The native client receives and manages the OAuth tokens and returns to a connected state.
4. Only after authentication, discover the actual tool schemas and verify with a read-only list of projects or tools. Do not generate, publish, send, or edit anything as a connection check.

## Custom API host / staging

The standard native registration commands accept a host through the endpoint URL. Use HTTPS, or HTTP only for localhost / numeric loopback. Append `/api/v2/mcp` to the API origin. Validate that an origin has no credentials, query, fragment or extra path. Use distinct names such as `casually-stg` and `casually-local`.

```sh
claude mcp add --transport http --scope user casually-stg https://STAGING_API_HOST/api/v2/mcp
codex mcp add casually-stg --url https://STAGING_API_HOST/api/v2/mcp
```

Replace `STAGING_API_HOST` with the actual staging host; do not execute an example host. Authenticate that named server through `/mcp` or Codex’s MCP login. For a manually registered Codex connection, `codex mcp login casually-stg` is also supported. Disable the bundled production MCP server in the client’s MCP settings when doing staging work. Before calling tools, verify the selected server name and environment; never fall back to production when staging is unavailable.

Casually’s `/dashboard/plugins` guide automatically identifies the current API environment and displays its endpoint, connection name, instructions and ready-to-copy registration command. Do not ask the user to type a host already shown by that guide. Custom hosts remain available in its developer section. The guide also lists the user’s authorized connections. A user can revoke a connection there. Re-authenticate in the client after expiry or revocation. New OAuth support must be deployed on the selected API and Web hosts, including the database migration; MCP API keys have been discontinued, including previously issued ones. REST API keys remain available for system integrations and cannot authenticate MCP. Do not fall back to an API key.

Claude Desktop and claude.ai custom connectors can use the same HTTPS MCP URL and browser consent flow. Plugin installation commands here are for Codex and Claude Code.
