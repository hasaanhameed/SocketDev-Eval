# Socket.dev Evaluation

## Setup

Install with socket.dev CLI

```
npm install -g socket
```

Verify installation using: `socket --version`
Result: `1.1.143` — installed successfully.

### Setup the token

Go to socket.dev and sign up.

After sign up, create an organization without GitHub (if you don't want to use the GitHub app).

![Create organization](screenshots/setup/01-create-org.png)

After the organization is created, you'll be redirected to the main dashboard.

![Dashboard](screenshots/setup/02-dashboard.png)

After that, click on the org name, go to Settings, and under Integrations go to API Tokens, then create a new API token.

![API tokens](screenshots/setup/03-api-tokens.png)

You'll have the following access (scope) options for that API token.

![Token scopes](screenshots/setup/04-token-scopes.png)

For experimentation purposes, you can select all scopes and proceed.

Copy the token value and save it in an environment file (`.env` — not committed to this repo).

Now authenticate the CLI using: `socket login`

It will ask for your token — paste it.

Setup is complete.

![Login complete](screenshots/setup/05-login-complete.png)

### Notes

- Note: `docs.socket.dev/docs/api-keys` (linked from the CLI prompt) currently 404s — use the
  main `socket.dev` dashboard instead to create tokens.
- The API token's quota shown on the token page (e.g. 500) is a per-token allocation, separate
  from the org's overall monthly plan quota (Free tier = 1,000 scans/month).
