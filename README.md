# TSP MCP

Internal Trilogy Search MCP server. Sends text, Markdown, or HTML to the signed-in user's Microsoft Teams "Note to self" chat via Microsoft Graph delegated auth.

Three plugin entries in the marketplace:

- `tsp-mcp` — hosted at `https://mcp.trilogy-search.com` (any OS, no local binary). **Recommended.**
- `tsp-mcp-windows` — local binary, Windows x64.
- `tsp-mcp-macos` — local binary, macOS Apple Silicon.

`github.com/trilogy-search/tsp-plugin` is the Claude plugin marketplace/distribution repo (the shorthand `trilogy-search/tsp-plugin` works on `claude plugin marketplace add`). This source repo (`github.com/trilogy-search/tsp-mcp`) is where the manifests point as their `repository`.

## Install — Hosted (recommended)

Works on any OS. Auth via Microsoft sign-in; no local binary.

```bash
claude plugin marketplace add trilogy-search/tsp-plugin
claude plugin install tsp-mcp@trilogy-search
```

The first `send_to_self_chat` call returns a one-time Entra consent URL. Open it, approve, retry the tool. After that, every machine you install on for the same user skips both consents (the encrypted refresh token is stored server-side).

## Install — Local plugin (offline fallback)

Use this if you need to operate without the hosted service. Otherwise prefer the hosted plugin above.

On macOS Apple Silicon:

```bash
claude plugin marketplace add trilogy-search/tsp-plugin
claude plugin install tsp-mcp-macos@trilogy-search
```

On Windows x64:

```powershell
claude plugin marketplace add trilogy-search/tsp-plugin
claude plugin install tsp-mcp-windows@trilogy-search
```

## Validate

```bash
claude plugin validate .
claude plugin validate plugins/tsp-mcp-hosted
claude plugin validate plugins/tsp-mcp-windows
claude plugin validate plugins/tsp-mcp-macos
```

## Rebuild Local Binaries

```bash
GOOS=windows GOARCH=amd64 go build -trimpath -ldflags='-s -w' -o plugins/tsp-mcp-windows/bin/tsp-mcp-windows-amd64.exe ./cmd/tsp-mcp
GOOS=darwin GOARCH=arm64 go build -trimpath -ldflags='-s -w' -o plugins/tsp-mcp-macos/bin/tsp-mcp-darwin-arm64 ./cmd/tsp-mcp
```

The hosted plugin has no binary — `scripts/export-plugin-repo.sh` skips it during cross-compilation.

## Notes

The local plugin manifests include the Microsoft Entra client ID and tenant ID. These are public identifiers for a native/public-client app, not secrets. Do not commit client secrets or user tokens.
