# ThoughtMint for Claude

This plugin connects Claude to your ThoughtMint account and adds three skills for LinkedIn writing. ThoughtMint writes LinkedIn posts and carousels in your own voice, from the profile you set up in ThoughtMint. With the plugin, you can plan a week of posts, turn a blog article into a post, or write a carousel's slide text without leaving Claude.

You need a ThoughtMint account (a 7-day free trial or a paid plan). Sign up at [thoughtmint.ai](https://thoughtmint.ai).

## Skills

| Skill | What it does |
|---|---|
| `weekly-linkedin-plan` | Reads your profile, saved ideas and calendar, proposes posts for the week, writes the ones you approve, and puts them on your ThoughtMint calendar when you say so. |
| `post-from-blog-url` | Reads an article you link to, writes a LinkedIn post from it as a ThoughtMint draft, and offers to schedule it. |
| `carousel-from-idea` | Writes the slide text and caption for a LinkedIn carousel. You design and post the carousel in the ThoughtMint app. |

You can also ask Claude to use the ThoughtMint tools directly, for example "show my saved ideas" or "how many words do I have left this month?".

## What it runs and what it sends

- The plugin contains no code. It has three skills (instructions for Claude) and one connection: the ThoughtMint connector at `https://thoughtmint.ai/mcp`.
- The first time Claude uses ThoughtMint, you sign in on thoughtmint.ai and approve the connection. Claude never sees your password. You can disconnect it at any time in ThoughtMint under Settings → Connected apps.
- When a skill calls a ThoughtMint tool, ThoughtMint receives only what Claude passes to that tool: for example the idea for a post, the text of the article you asked about, a draft id, or the time to schedule a post. It does not receive your conversation, Claude's memory or your files.
- To write text, ThoughtMint sends the tool input and your profile details to OpenAI through its API. ThoughtMint does not use your content to train AI models.
- Writing tools use words from your ThoughtMint plan's monthly allowance. Reading, saving ideas and scheduling don't.
- For a blog article, Claude reads the web page with its own web tools before calling ThoughtMint.
- Scheduling: a scheduled text post is published to LinkedIn at that time only if your LinkedIn profile is connected in ThoughtMint; otherwise it stays on your calendar as a plan. Carousels are never published automatically. ThoughtMint emails you for every post scheduled from Claude, with a link that cancels it. The skills confirm each post and its time with you before scheduling it.
- A `source_url` passed to ThoughtMint is stored with the draft as its source. ThoughtMint doesn't fetch it.

## Install

Install it from Claude's plugin directory. To try it from a copy of this repository in Claude Code:

```
claude --plugin-dir ./thoughtmint-claude-plugin
```

Or connect ThoughtMint on its own in Claude: Settings → Connectors → add `https://thoughtmint.ai/mcp`. Setup steps and example prompts: [thoughtmint.ai/claude](https://thoughtmint.ai/claude/).

## Support and policies

- Help: [info@thoughtmint.ai](mailto:info@thoughtmint.ai)
- Privacy policy: [thoughtmint.ai/privacy](https://thoughtmint.ai/privacy/)
- Terms: [thoughtmint.ai/terms](https://thoughtmint.ai/terms/)

## License

MIT. See [LICENSE](LICENSE).
