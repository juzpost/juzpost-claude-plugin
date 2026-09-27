---
name: social-media-scheduler
description: Draft, schedule and publish social media posts to YouTube, Instagram, Facebook, TikTok, X, LinkedIn, Pinterest, Bluesky, Threads, Tumblr and WordPress with the JuzPost tools, confirming with the user before anything is published.
---

# Scheduling posts with JuzPost

Use the JuzPost MCP tools to turn a brief into a scheduled post. Creating and scheduling need the Pro plan on the current workspace; reading needs Solo or higher. If a tool isn't connected yet, see the `setup` skill.

## The flow

1. **Check the connection.** Call `get_context`. It returns the workspace, plan, timezone, default posting times and anything blocked. If creating or scheduling is blocked, tell the user what the plan allows before drafting.
2. **Pick the accounts.** Call `list_accounts` for account ids, platforms, usernames and health. If the user names a group ("my brand accounts"), call `list_groups` to resolve it. Skip accounts marked as needing reconnection and say which ones.
3. **Check the fit.** For captions, titles or media near a limit, call `get_platform_limits` for the platforms involved.
4. **Attach media.** For a file or a link, call `upload_media` (a public URL up to 100 MB, or base64 up to 3 MB) and keep the returned `media_key`. `create_post` also accepts public URLs directly in `media_urls`.
5. **Create the draft.** Call `create_post` with `type` set to the type the user asked for (`text`, `image` or `video`). If the user hasn't said, ask. Include `content`, and `title` for YouTube and Pinterest. Pass an `idempotency_key` so a retry never creates a second draft. The response lists warnings for any platform whose caption limit is exceeded.
6. **Confirm.** Show the user the caption, media, the accounts and the time (in the workspace timezone). When a Bluesky account gets images or video, ask for alt text for each file, and whether the media needs a content warning. Do not schedule until the user agrees.
7. **Schedule or publish.** Call `schedule_post` with `post_id`, `account_ids`, and either `publish_at` (ISO 8601 UTC, at least 10 minutes ahead) or `publish_now: true`. Use `platform_overrides` for per-account captions and `publish_at_overrides` for per-account times. For Bluesky accounts, pass `bluesky_settings` keyed by account id: `alt_text` with one entry per file in the post's order (`""` for a file with no description, up to 2,000 characters each) and `labels` (at most one of `sexual`, `nudity`, `porn`, plus `graphic-media`, or `[]`). Labels only go on posts with images or video. Retrying the same call is safe: accounts already scheduled are not scheduled twice.
8. **Report.** Call `get_post` to show each account's status. After publishing, `get_post` also explains any failure for an account.

## Working with existing posts

- `list_posts` filters by `status` (`draft`, `scheduled`, `published`, `failed`), account and date range.
- `update_post` edits a draft before it is scheduled. Scheduled and published posts can't be edited.
- `delete_post` deletes a draft. Confirm with the user first.

## Limits to keep in mind

- One post per `create_post` call; there is no bulk creation.
- A workspace can create 30 posts per hour through a connector.
- The Free plan schedules up to 30 days ahead and does not post to X; paid plans schedule up to 180 days ahead.
- A plan lock error names the plan the action needs and links to https://www.juzpost.com/pricing. Pass that on to the user as it is.
