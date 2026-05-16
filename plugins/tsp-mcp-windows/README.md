# Teams Self Chat for Windows x64

Claude Code plugin that sends text, Markdown, or HTML to the signed-in user's Microsoft Teams self-chat through Microsoft Graph.

The first send opens Microsoft sign-in in the browser. The token is persisted to the OS credential store (Windows Credential Manager) with a `0600`-mode JSON file fallback, so subsequent Claude Code sessions reuse the existing sign-in. Set `TSP_MCP_SIGN_OUT=true` on startup to delete the cached token and force fresh consent.
