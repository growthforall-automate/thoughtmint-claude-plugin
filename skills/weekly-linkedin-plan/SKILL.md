---
name: weekly-linkedin-plan
description: Plan a week of LinkedIn posts with the user's ThoughtMint account, write the posts they approve in their voice, and put them on their ThoughtMint calendar. Use when the user asks to plan their LinkedIn week, a posting schedule, or several posts for the coming days.
---

# Weekly LinkedIn plan

Plan the user's LinkedIn posts for the coming week with the ThoughtMint connector, write the ones they approve, and schedule them. The tools below are the ThoughtMint connector's; their names may carry a prefix in your tool list.

If the ThoughtMint tools aren't available, tell the user to connect ThoughtMint (setup steps: https://thoughtmint.ai/claude/) and stop.

## 1. Check what's available

1. Call `get_account_status`. Tell the user how many words they have left this month. Writing a post uses about as many words as the post itself (a medium post is roughly 150–300 words); revisions and new hooks use more. If they have fewer words than the week needs, say so and suggest fewer or shorter posts.
2. Call `get_voice_profile` to learn their role, audience and content pillars.
3. Call `list_ideas` to see their saved ideas.
4. Call `list_posts` with `status: "scheduled"` to see what is already on their calendar, so the plan doesn't clash with it.

## 2. Agree the plan

1. Ask how many posts they want this week (suggest 3) and on which days and times. Ask for their time zone if you don't know it: scheduling needs a time with a UTC offset.
2. Propose one line per post: day and time, the idea, and the angle. Prefer their saved ideas and their content pillars. If they have too few ideas, call `generate_ideas` with a topic from their pillars (this uses words) or ask them for topics.
3. Wait for the user to approve or change the plan before writing anything.

## 3. Write the posts

For each approved post:

1. Call `write_post` with the idea, including any specifics, stories or numbers the user gave you. Pass `length` if they asked for short or long posts.
2. Show the post. If the result lists `gaps` (placeholders like `[the result]`), ask the user each `question` and call `refine_post` with `gap_answers`. A post with placeholders can't be scheduled.
3. If the user wants changes, call `refine_post` with an `instruction` or a `preset`. If they want different opening lines, call `suggest_hooks`.

Each call to `write_post`, `refine_post`, `suggest_hooks` and `generate_ideas` uses words from their allowance.

## 4. Schedule

1. Confirm the final list with the user: each post's day and time. Tell them before you schedule: if their LinkedIn account is connected in ThoughtMint, each text post is published to LinkedIn automatically at its time; if it isn't, the posts stay on their ThoughtMint calendar as plans.
2. For each post they approve, call `schedule_post` with its `draft_id` and `at` as an ISO 8601 date and time with their UTC offset (for example `2026-12-01T09:00:00+05:30`). Times must be at least 15 minutes and at most a year ahead.
3. Report the result for each post:
   - if `goes_out_automatically` is true, it will be published to their LinkedIn at that time;
   - if false, it stays on their ThoughtMint calendar as a plan and they post it themselves; `why_not` says why;
   - ThoughtMint emails them a link that cancels the post. If `email_sent` is false, pass on the `email_note`.
4. If they change a scheduled draft later with `refine_post`, call `schedule_post` again with the same `draft_id` so the calendar gets the new text.

To take a post off the calendar, call `unschedule_post` with its `post_id`.

## Finish

Summarise the week: each post's day and time, its first line, whether it auto-publishes, and the `open_in_thoughtmint` link for editing in the app.
