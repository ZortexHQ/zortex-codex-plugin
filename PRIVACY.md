# Zortex Privacy Policy

Effective: September 10, 2026

Zortex is a local-first context system. When you ask Zortex to connect Google,
it requests read-only access to Gmail and Google Drive so it can synchronize the
content you authorized into your local Zortex vault and answer your requests
with citations.

## Data use and storage

- Gmail and Drive content is read only for the connection, synchronization, and
  analysis you request. Zortex does not send, edit, or delete Google content.
- Synced content and OAuth tokens are stored locally in encrypted Zortex custody.
- A Cloudflare Worker adds the Google OAuth client secret during token exchange.
  It does not store or log authorization codes, tokens, or Google content.
- Selected cited context may be sent to the AI provider you choose when you ask
  that provider to analyze your connected data, subject to that provider's terms.
- Zortex does not sell Google user data or use it for advertising.

## Sharing, retention, and deletion

Zortex does not share Google user data except with the infrastructure needed to
complete OAuth and with the AI provider you explicitly use for analysis. Local
data remains until you revoke the connection or use Zortex's audited privacy
erasure path. Revoking access in your Google Account prevents future access.

## Contact

Questions or deletion requests: jpl223705@gmail.com
