---
name: setup
description: Connect JuzPost to Claude, pick the workspace, and fix sign-in, workspace or plan problems with the JuzPost tools.
---

# JuzPost setup

This plugin adds the JuzPost MCP server (`https://www.juzpost.com/mcp`). JuzPost is a social media scheduler for YouTube, Instagram, Facebook, TikTok, X, LinkedIn, Pinterest, Bluesky, Threads, Tumblr and WordPress.

## Connecting

1. The first time a JuzPost tool runs, Claude opens a JuzPost sign-in page in the browser. Sign in or create an account at https://www.juzpost.com.
2. On the consent screen, choose the workspace the connection should act on and the permissions to grant.
3. After approval, the JuzPost tools are available. In Claude Code, `/mcp` shows the `juzpost` server and its status.

No API key, client id or secret is needed. To disconnect, revoke the connection in JuzPost under Settings, Connections.

## Checking what the connection can do

- `get_context` reports the signed-in user, the current workspace, its plan, quotas, and a plain-language list of anything blocked right now.
- `list_workspaces` lists every workspace the user belongs to and what each one allows: `none`, `read` or `full`.
- `switch_workspace` moves the connection to another workspace by id or slug. Confirm the workspace with the user first.

Listing and switching workspaces work on every plan, so a connection on a locked workspace can always move to one that allows more.

## What each plan allows through Claude

Access follows the plan of the workspace the connection points at.

| Plan | Price | Through Claude |
|---|---|---|
| Free | $0 | List and switch workspaces only |
| Solo | $15/month | Read: context, accounts, groups, posts, platform limits |
| Growth | $30/month | Read: context, accounts, groups, posts, platform limits |
| Pro | $69/month | Everything: upload media, create, edit and delete drafts, schedule and publish |

Current prices and limits: https://www.juzpost.com/pricing

## Troubleshooting

| What happens | What it means | What to do |
|---|---|---|
| A sign-in page opens again | The connection expired or was revoked | Sign in again; nothing else is lost |
| "needs the Solo plan or higher" or "needs the Pro plan or higher" | The current workspace's plan does not include that action | Tell the user which plan the action needs, or check `list_workspaces` for a workspace that allows it |
| "no longer a member" | The user was removed from the workspace | Use `list_workspaces`, then `switch_workspace` |
| A rate limit message about posts in the last hour | A workspace can create 30 posts per hour through a connector | Wait for the hour to pass, or schedule drafts that already exist |
| A tool is not listed | The permission for it was not granted at sign-in | Disconnect under Settings, Connections and connect again with that permission |

## Links

- Tool reference: https://api.juzpost.com/docs
- Privacy policy: https://www.juzpost.com/privacy-policy
- Terms: https://www.juzpost.com/terms-of-service
- Support: support@juzpost.com or https://www.juzpost.com/support
