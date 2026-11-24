---
name: carousel-from-idea
description: Write the slide-by-slide text and caption for a LinkedIn carousel in the user's voice with their ThoughtMint account, then explain how to design and post it in ThoughtMint. Use when the user asks for a LinkedIn carousel, slides or a document post on a topic.
---

# Carousel from an idea

Write a LinkedIn carousel's text with the ThoughtMint connector. The tools below are the ThoughtMint connector's; their names may carry a prefix in your tool list.

If the ThoughtMint tools aren't available, tell the user to connect ThoughtMint (setup steps: https://thoughtmint.ai/claude/) and stop.

## 1. Shape the idea

1. Make sure you know what the carousel is about and who it is for. If the user gave only a topic, ask one question about the angle or the point they want to make.
2. If they have source material (notes, an article they pasted, a post of theirs), you'll pass it as `source`. To start from one of their posts, find it with `list_posts` and read it with `get_post`.
3. Agree the number of slides if they care (4–12; the default is 9). If they want a particular storyline, choose an `arc` such as `how_to`, `mistakes`, `story` or `framework`; otherwise leave it out and ThoughtMint picks.

## 2. Write it

1. Call `write_carousel` with `idea`, and `slide_count`, `arc` and `source` as agreed. It uses words from their ThoughtMint allowance and can take about 20–30 seconds.
2. Show each slide's title and text in order, then the `caption`. Mention `why_this_arc` in one line. If `notes` is not empty, pass the notes on.
3. If they want changes, either edit the text with them directly, or call `write_carousel` again with a sharper idea or a different `arc`. Calling it again uses words again, so say so first.

## 3. Design and post it

`write_carousel` returns text only: it doesn't design the carousel or save it in ThoughtMint, and it can't be scheduled from here. Tell the user how to finish it:

1. Open ThoughtMint (https://thoughtmint.ai) and choose **Create carousel**.
2. Use this slide text in the editor, pick a design, and export the PDF.
3. Post the PDF on LinkedIn as a document post, with the caption. If they want it on their ThoughtMint calendar, they can schedule it from the app; carousels are never published automatically.
