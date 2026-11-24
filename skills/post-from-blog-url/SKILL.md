---
name: post-from-blog-url
description: Turn a blog post or article URL into a LinkedIn post in the user's voice with their ThoughtMint account, saved as a ThoughtMint draft, and optionally schedule it. Use when the user shares a link and asks for a LinkedIn post from it, or asks to repurpose an article for LinkedIn.
---

# LinkedIn post from a blog URL

Write a LinkedIn post from an article with the ThoughtMint connector. The tools below are the ThoughtMint connector's; their names may carry a prefix in your tool list.

If the ThoughtMint tools aren't available, tell the user to connect ThoughtMint (setup steps: https://thoughtmint.ai/claude/) and stop.

## 1. Read the article

1. Read the page with your own web tools. If you can't open it (paywall, login, blocked), ask the user to paste the text.
2. Pull out the main point, the two or three strongest specifics (numbers, examples, quotes, steps) and who it is for.
3. Ask the user, in one short question, what they want readers to take away or do, unless they already said. Ask whether it is their own article or someone else's: the post should credit the author when it isn't theirs.

## 2. Write the post

1. Call `write_post`:
   - `idea`: the angle in a sentence or two, then the specifics you pulled out and the takeaway. Keep it under 4,000 characters; don't paste the whole article.
   - `source_url`: the article's URL.
   - `length` and `format` only if the user asked for them.
   This uses words from their ThoughtMint allowance.
2. Show the post and its `alternative_hooks`.
3. If the result lists `gaps` (placeholders like `[your result]`), ask the user each `question` and call `refine_post` with `gap_answers`.
4. For changes, call `refine_post` with an `instruction` or a `preset`. For new opening lines, call `suggest_hooks`. Each of these uses words.

## 3. Offer to schedule

1. Ask whether they want to schedule it, and when. Ask for their time zone if you don't know it.
2. If they do, call `schedule_post` with the `draft_id` and `at` as an ISO 8601 date and time with their UTC offset (for example `2026-12-01T09:00:00+05:30`), at least 15 minutes and at most a year ahead.
3. Tell them what happens: if `auto_publish` is true it is published to their LinkedIn at that time; otherwise it stays on their ThoughtMint calendar as a plan. ThoughtMint emails them a link that cancels it; if `email_sent` is false, pass on the `email_note`.

Otherwise, give them the `open_in_thoughtmint` link to edit or post it from the app.
