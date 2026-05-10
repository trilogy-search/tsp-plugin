# Trilogy Teams Plugin

Claude plugin marketplace for sending agent output to the signed-in user's Microsoft Teams self-chat.

## Plugins

This repository exposes two Claude plugin entries:

- `teams-self-chat-windows` for Windows x64
- `teams-self-chat-macos` for macOS Apple Silicon

Both plugins run the same MCP server and send through Microsoft Graph to `48:notes`, which resolves to the signed-in delegated user's Teams self-chat.

## Install In Claude Code

Prerequisites:

- Claude Code is installed and available as `claude`.
- You can sign in to the Trilogy Microsoft tenant in a browser.
- Your machine is one of the supported platforms below:
  - Windows x64
  - macOS Apple Silicon

Add this plugin marketplace once:

```bash
claude plugin marketplace add github:trilogy-search/tsp-plugin
```

Then install the plugin for your platform.

On macOS Apple Silicon:

```bash
claude plugin install teams-self-chat-macos@trilogy-search
```

On Windows x64:

```powershell
claude plugin install teams-self-chat-windows@trilogy-search
```

## Install In Claude Cowork

Prerequisites:

- Claude Desktop with Cowork access on a paid Claude plan.
- You can sign in to the Trilogy Microsoft tenant in a browser.
- Your machine is one of the supported platforms:
  - Windows x64
  - macOS Apple Silicon

Install from the Cowork UI:

1. Open Claude Desktop.
2. Switch to the `Cowork` tab.
3. Open `Customize` from the left sidebar.
4. Click `Browse plugins`.
5. Select `Personal`.
6. Click `+`, then choose `Add marketplace from GitHub`.
7. Enter `https://github.com/trilogy-search/tsp-plugin`.
8. Install the plugin for your platform:
   - `teams-self-chat-windows` for Windows x64
   - `teams-self-chat-macos` for macOS Apple Silicon

This plugin includes a local MCP server binary that runs on your machine. If your organization manages Claude through an Enterprise policy, local MCP servers may be restricted or disabled by your admin.

## First Use

The first time the plugin sends a message, it opens Microsoft sign-in in your browser. Sign in with your Trilogy Microsoft account and accept any required permissions. Tokens are kept in memory only, so after restarting Claude Code or Claude Desktop you may need to sign in again.

## Manage In Claude Code

List installed plugins:

```bash
claude plugin list
```

You should see either `teams-self-chat-macos@trilogy-search` or `teams-self-chat-windows@trilogy-search`, depending on your platform.

Update the marketplace and reinstall the platform plugin:

On macOS Apple Silicon:

```bash
claude plugin marketplace update trilogy-search
claude plugin install teams-self-chat-macos@trilogy-search
```

On Windows x64, use:

```powershell
claude plugin marketplace update trilogy-search
claude plugin install teams-self-chat-windows@trilogy-search
```

Remove the plugin:

```bash
claude plugin uninstall teams-self-chat-macos@trilogy-search
```

On Windows x64, use:

```powershell
claude plugin uninstall teams-self-chat-windows@trilogy-search
```

## Manage In Claude Cowork

Open Claude Desktop, switch to `Cowork`, then go to `Customize` to view, update, or remove installed plugins.

## Validate

For maintainers:

```bash
claude plugin validate .
claude plugin validate plugins/teams-self-chat-windows
claude plugin validate plugins/teams-self-chat-macos
```

## Rebuild Binaries

For maintainers:

```bash
GOOS=windows GOARCH=amd64 go build -trimpath -ldflags='-s -w' -o plugins/teams-self-chat-windows/bin/teams-mcp-windows-amd64.exe ./cmd/teams-mcp
GOOS=darwin GOARCH=arm64 go build -trimpath -ldflags='-s -w' -o plugins/teams-self-chat-macos/bin/teams-mcp-darwin-arm64 ./cmd/teams-mcp
```

## Notes

The plugin manifests include the Microsoft Entra client ID and tenant ID. These are public identifiers for a native/public-client app, not secrets. Do not commit client secrets or user tokens.
