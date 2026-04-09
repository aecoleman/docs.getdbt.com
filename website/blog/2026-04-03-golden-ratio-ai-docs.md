---
title: "From browser to prompt"
description: "A dbt docs writer on why human-curated docs matters more than ever in this AI era and how we brought docs.getdbt.com into your AI workflow."
slug: from-browser-to-prompt
authors: [mirna_wong]
tags: [ai, docs]
hide_table_of_contents: false
date: 2026-04-03
is_featured: true
---

For most of my career as a technical writer, success looked like page views. Someone searches for a dbt concept, lands on docs.getdbt.com, reads the page. Job done.

So when I started seeing that traffic decline, my first instinct was to worry. My second instinct was to go find out why. What I found reframed how I think about docs entirely — and led directly to what we just shipped.

Earlier this year I found myself pulling together a research doc to answer a simple question: where do dbt users actually go when they need help? I used dbt platform to build out the analysis—querying MCP server telemetry, looking at tool event volume, comparing it against docs traffic patterns. What I found reframed how I think about docs entirely.

Direct doc visits to docs.getdbt.com are declining. Meanwhile, the dbt MCP server—which lets AI assistants work directly with your dbt environment—had 355,198 tool events in just the first two months of 2026. That's more than all of 2025 combined (337,236). Users aren't ignoring the docs. They're getting answers a different way.

That research led directly to what we just shipped: product docs fetch capability in the dbt MCP server. Here's how we got there.

<!-- truncate -->

## What's the dbt MCP server?

MCP stands for Model Context Protocol—an open standard (now backed by Anthropic, OpenAI, Google, and Microsoft) that lets AI assistants connect to external tools and data sources. Instead of an AI approximating an answer from training data, it can reach out, pull real information, and respond with actual context.

The dbt MCP server connects AI assistants to your dbt environment. Ask Claude about a model's lineage, a failing test, or what a macro does—and it can look it up in your real project. You can run it locally (in VS Code, your terminal) or connect it remotely to query a shared team project via dbt Platform.

Before this release, the dbt MCP server had eight toolset categories: CLI, Semantic Layer, Discovery, Admin API, SQL, Codegen, Fusion, Server Metadata. Not one of them could access docs.getdbt.com. When an agent needed to answer "how do I configure incremental models?"—it was on its own.

## The research phase: what the data showed

I queried our MCP server telemetry in dbt Platform and the numbers told a clear story about how fast this is growing:

| | 2025 (full year) | 2026 (Jan–Feb only) |
|---|---|---|
| Total tool events | 337,236 | 355,198 |
| Local MCP | 118,278 | 208,418 |
| Remote MCP | 101,477 | 136,444 |

Local MCP usage nearly doubled in two months. Remote MCP grew 34%. And across it all—**2,353 unique accounts and 1,319 users** connected and actively using the server.

That last number mattered a lot to the decision we were about to make.

## The decision: why MCP, and why this MCP

We already had some pieces in place for AI-readable docs:

- `docs.getdbt.com/llms.txt` — a page index for AI systems
- `docs.getdbt.com/llms-full.txt` — full docs content in one file
- `.md` suffix support on any docs page for clean markdown output
- A published `fetching-dbt-docs` skill that teaches AI agents how to access our docs via web requests

The skill approach worked, but it had a friction problem: the agent needed to know the skill existed, install it, and perform web fetches as a workaround. About 56 installs per week—not bad, but a fraction of the MCP server's reach.

Adding docs tools directly to the MCP server means existing users get the capability automatically, with no action required on their part. 2,353 accounts versus 56 weekly installs is not a close comparison.

We also considered whether to build a separate, standalone docs MCP server—something like what OpenAI and Microsoft have done with their documentation. But their situation is different: massive, sprawling doc surfaces across dozens of unrelated products. For a single coherent product like dbt, adding docs as a ninth toolset category in the existing server made far more sense. One server to maintain. No new infrastructure. And docs available in the same context as project work, right when someone is mid-workflow and needs an answer.

Google's move to expose their docs via API was an early signal that the industry was heading this direction. By the time we were making this call, OpenAI, Microsoft, GitBook, and Mintlify had all made similar investments. MCP is becoming the standard for AI-tool integration across the board, and our work fits into that ecosystem naturally.

## Why human-curated docs matter more now, not less

I want to be honest about something that surprised me in my own research: the fact that AI assistants are displacing direct doc visits isn't a problem. It's actually validation.

But it does raise the stakes.

When an AI assistant answers a dbt question from its training data, you get an approximation—maybe accurate, maybe slightly outdated, maybe confidently wrong about a feature that changed six months ago. When it fetches directly from docs.getdbt.com, you get the current, canonical answer. The one a human wrote, reviewed, and maintained.

As the docs team at dbt Labs, our content is open source—anyone can contribute, and the accuracy of what's on docs.getdbt.com reflects the collective care of our team and community. In an AI-assisted world, that work doesn't matter less. It gets amplified. Every gap in the docs becomes a gap in AI answers at scale. Every inaccuracy propagates further than it ever could when someone had to navigate to a page themselves.

AI without good docs is guessing at best, hallucinating at worst. Good docs without AI access is friction. But together, they're actually better than either alone &mdash; and that's the shift this blog post is pointing at. It used to be: open a browser, find the page, read it, go back to work. Now it's: ask a question, get the answer, keep going. Same docs. Different path.

## What we shipped

We added two tools to the dbt MCP server under a new "Product Docs" category:

- **`search_product_docs`** — searches docs.getdbt.com and returns titles, URLs, and relevance-ranked descriptions for pages matching your query.
- **`get_product_doc_pages`** — fetches the full markdown content of one or more docs pages by path or URL.

The workflow mirrors how a human would use the docs: search first to find what's relevant, then fetch the full content. The difference is it happens inside whatever AI tool you're already using, without a context switch.

Because these tools hit public URLs, they work without a dbt Cloud account. That means every dbt user—open source or Platform—gets docs access through MCP for free.

## What this changes

The practical difference is staying in flow. Before: write a macro, hit an unfamiliar function, alt-tab to a browser, find the docs page, read it, return to your editor, try to remember where you were. After: ask Claude directly, get an answer grounded in actual, current documentation, keep coding.

For analysts exploring a shared project, it means understanding what a model does without navigating to a separate tab. For teams working across different dbt setups—dbt Cloud, dbt Core, dbt Platform—it means consistent, authoritative answers regardless of where they're working or who's asking.

For the docs team, it means the work we put into writing and maintaining docs.getdbt.com is now doing more than it was before.

## Try it

If you're already connected to the dbt MCP server, the product docs tools are available now. See the [MCP available tools reference](https://docs.getdbt.com/docs/dbt-ai/mcp-available-tools?version=2.0#product-docs) for details on `search_product_docs` and `get_product_doc_pages`.

If you're new to the MCP server, the [setup guide](https://docs.getdbt.com/docs/dbt-ai/mcp-available-tools) walks through both local and remote configuration.

And if you have thoughts on how it's working—or what you wish it could do—I'd genuinely like to hear them. This whole thing started from paying attention to how people actually use dbt. That feedback loop is how we figure out what to build next.

---

*Mirna Wong is a technical writer at dbt Labs. dbt's documentation is open source—contributions and issues are always welcome at [github.com/dbt-labs/docs.getdbt.com](https://github.com/dbt-labs/docs.getdbt.com).*
