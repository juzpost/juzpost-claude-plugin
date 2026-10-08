---
name: post
description: Draft and schedule one social media post with JuzPost from a short brief, with confirmation before it is scheduled.
---

# /juzpost:post

Usage: `/juzpost:post <what to post, where and when>`

Example: `/juzpost:post Video ./launch.mp4 to YouTube and TikTok tomorrow at 9am, caption "We just shipped dark mode"`

Follow the `social-media-scheduler` skill with the brief given after the command:

1. Read the brief for the post type, text, media, accounts and time. Ask for anything missing, including the post type if it isn't stated.
2. Show the caption, media, accounts and time in the workspace timezone, and wait for the user to confirm.
3. Schedule with one `schedule_post` call, then report each account's status with `get_post`.
