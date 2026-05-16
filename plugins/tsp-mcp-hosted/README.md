# Teams Self Chat — Hosted

Claude Code plugin that sends text, Markdown, or HTML to the signed-in user's Microsoft Teams "Note to self" chat through Microsoft Graph. Works on any OS — no local binary, no Go toolchain.

## First-call walkthrough

Two consent screens, both one-time per user.

1. **Microsoft sign-in.** The first time Claude Code talks to the hosted MCP, a browser opens to the Cognito hosted UI, which immediately redirects to the Trilogy Entra sign-in. On a corporate-managed laptop with active SSO, this is usually a single click.
2. **Graph delegation consent.** On your first `send_to_self_chat` call, the tool returns an `auth_required` result with a one-time Entra consent URL. Click it, approve the `Chat.ReadWrite` scope, retry the tool. The signed message lands in your Teams self-chat within a second or two.

After both consents, your encrypted Entra refresh token is stored server-side (KMS-sealed in DynamoDB). Any other machine you install this plugin on signs you in silently and reuses the stored token — no second consent prompt.

## Sign out / reset

To force a fresh consent flow (e.g. switching Trilogy accounts), have an operator run `cognito-idp:AdminDeleteUser` on your Cognito user and delete your row in `tsp-mcp-users`. The next tool call will go through both consent screens again.
