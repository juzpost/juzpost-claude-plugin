---
name: queue
description: Show the JuzPost queue: scheduled, failed and draft posts, with the reason for any failure.
---

# /juzpost:queue

Usage: `/juzpost:queue [scheduled|failed|draft|published]`

1. Call `list_posts` with `status` set to the filter given, or once each for `scheduled`, `failed` and `draft` when none is given. For scheduled posts, sort by `scheduledFor` ascending.
2. Show each post's caption (first line), accounts, and scheduled or published time in the workspace timezone from `get_context`.
3. For each failed post, call `get_post` and give the per-account error in plain language.
4. If a list has more pages, say so and offer to continue.
