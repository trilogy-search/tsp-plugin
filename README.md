# Trilogy Teams MCP

Claude Code MCP server and plugin marketplace for sending agent output to the signed-in user's Microsoft Teams self-chat.

## Plugins

This repository exposes two Claude Code plugin entries:

- `teams-self-chat-windows` for Windows x64
- `teams-self-chat-macos` for macOS Apple Silicon

Both plugins run the same MCP server and send through Microsoft Graph to `48:notes`, which resolves to the signed-in delegated user's Teams self-chat.

## Install From GitHub

The repository can be public or private. For a private repository, each installer needs GitHub access to the repo from their machine.

```bash
claude plugin marketplace add github:trilogy-search/tsp-plugin
claude plugin install teams-self-chat-macos@trilogy-search
```

On Windows x64:

```powershell
claude plugin marketplace add github:trilogy-search/tsp-plugin
claude plugin install teams-self-chat-windows@trilogy-search
```

## Validate

```bash
claude plugin validate .
claude plugin validate plugins/teams-self-chat-windows
claude plugin validate plugins/teams-self-chat-macos
```

## Rebuild Binaries

```bash
GOOS=windows GOARCH=amd64 go build -trimpath -ldflags='-s -w' -o plugins/teams-self-chat-windows/bin/teams-mcp-windows-amd64.exe ./cmd/teams-mcp
GOOS=darwin GOARCH=arm64 go build -trimpath -ldflags='-s -w' -o plugins/teams-self-chat-macos/bin/teams-mcp-darwin-arm64 ./cmd/teams-mcp
```

## Notes

The plugin manifests include the Microsoft Entra client ID and tenant ID. These are public identifiers for a native/public-client app, not secrets. Do not commit client secrets or user tokens.
