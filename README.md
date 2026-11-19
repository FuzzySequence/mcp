# Fuzzy MCP

Connect an MCP-compatible assistant to your own Fuzzy account. Fuzzy hosts the server; you do not need to run a local server or share an API password.

## Connecting

| Setting | Value |
| --- | --- |
| Server URL | `https://app-api.fuzzysequence.com/public-mcp` |
| Transport | Streamable HTTP |
| Authentication | OAuth authorization code with PKCE (S256) and dynamic client registration |
| Default scopes | `read offline_access` |
| Account | Your own eligible Fuzzy production account |

Add the server in your client's MCP or connector settings, start authentication, and complete the Fuzzy sign-in and consent page. Then ask: **Use Fuzzy to list my campaigns.**

The server URL is an authenticated API endpoint. Opening it directly in a browser returns a 401 challenge. Start sign-in from your MCP client and open the authorization URL it generates.

### Codex

Install the connection package from this repository's marketplace:

```sh
codex plugin marketplace add FuzzySequence/mcp
codex plugin add fuzzy-sequence@fuzzy-sequence
```

Complete the connection's OAuth authentication in the client. For clients configured without a plugin, add the server through Settings → MCP servers, or merge [clients/codex.toml](clients/codex.toml) into your existing configuration. For the directly configured server:

```sh
codex mcp login fuzzy --no-browser --scopes read,offline_access --oauth-client-registration dcr
```

Open the generated authorization link and authorize your own account. A recent Codex version is required for these CLI options.

### Claude Code

```sh
claude plugin marketplace add FuzzySequence/mcp
claude plugin install fuzzy-sequence@fuzzy-sequence
```

Use `/mcp` to authenticate the Fuzzy server. The bundled configuration requests `read offline_access`.

### Claude web or Desktop

Open Customize → Connectors → Add custom connector. Enter the server URL above, select OAuth sign-in, and choose **Register automatically** when asked how to identify the OAuth client. Complete Fuzzy sign-in and consent. Team or Enterprise administrators may need to add the connector before members can connect their own accounts.

### Other MCP clients

Use the same server URL with Streamable HTTP and OAuth/PKCE. The server advertises its authorization endpoints through MCP resource discovery. Select `read offline_access` when your client offers scope configuration. Client support and organization policies determine whether custom remote servers can be installed.

An agent can automate configuration only when its session has access to the client's configuration or setup tools. Otherwise, add the connection through the client's settings once; the agent can then use the connected tools.

## Read-only tools

| Tool | Purpose |
| --- | --- |
| `get_account_details` | Account and profile information |
| `get_identity` | Connected LinkedIn and email identities |
| `get_credit_balance` | Current credit balance |
| `list_campaigns` | Campaigns in your account |
| `get_campaign` | Details of one campaign |
| `get_campaign_metrics` | Campaign performance |
| `list_contacts` | Contacts in your account |
| `get_contact` | Details of one contact |
| `list_contact_lists` | Contact lists |
| `list_posts` | Saved LinkedIn posts |
| `list_inbox_contacts` | Inbox conversations |

The packaged connection starts with read access. Grant additional OAuth permissions to build campaign drafts, launch reviewed campaigns, publish saved LinkedIn drafts, or message an existing LinkedIn connection or conversation. Fuzzy shows the cost for approval before a paid action. See the [public MCP guide](https://getfuzzy.ai/mcp) for tools, permissions, and rates.

## Troubleshooting

- **401 in a browser:** use your client's Authenticate/Connect action; the raw server URL is not the login page.
- **Authentication required:** start a fresh OAuth sign-in for this server and account.
- **Account access denied:** verify that the signed-in Fuzzy account has active access to the relevant Fuzzy features.
- **The agent cannot install MCP servers:** use the client's MCP settings or a local coding session with setup tools.
- **A tool is missing:** check the connection's granted scopes and the available tools in your client.

Keep access tokens, refresh tokens, passwords and authorization callback URLs out of shared chats and issue reports. To report a failure, provide the client/version, failing step, HTTP status or error code, and a request ID when returned.

This repository contains connection metadata and instructions. Fuzzy hosts and operates the authenticated service. A marketplace package or registry entry does not grant access to anyone's account; each person authorizes their own connection.
