---
title: "Golden ratio: AI + Docs = harmonious flow state"
description: "Learn about the golden ratio of AI + Docs = harmonious flow state."
slug: golden-ratio-ai-docs
authors: [mirna_wong]
tags: [ai, docs]
hide_table_of_contents: false
date: 2026-04-03
is_featured: true
---

## Hook / Opening
[2-3 sentences establishing the friction: anyone working with dbt (developers, analysts, managers) context-switching between their tools and the browser, breaking flow state, losing momentum]

"We've just shipped docs.getdbt.com fetch capability to the dbt MCP server—and it changes how *anyone* working with dbt gets answers."

---

## What's the MCP Server? (Quick primer)
[1-2 paragraphs explaining MCP servers at a high level—no deep technical weeds]
- What it is (Model Context Protocol, enables AI assistants to interact with tools and data)
- Why dbt has one (lets Claude and other AI assistants understand your dbt projects in real time—locally or remotely)
- Who uses it (developers, analysts, teams querying shared dbt projects through AI assistants)

**Why this matters:** Anyone asking questions about dbt—whether you're building models, exploring data, or understanding lineage—now gets answers grounded in your actual dbt setup AND official, up-to-date docs.

---

## Behind the Decision: Why MCP + Docs, Not a Separate API?

[2-3 paragraphs explaining the research and decision-making process]

### The Research Phase
We started by asking: *Where do dbt users actually look for answers?* 

Through research on docs context patterns (analyzing where users go, what they search for, how they consume information), we discovered that dbt users are already using AI assistants (Claude, ChatGPT, etc.) to get help. The problem wasn't *finding* an answer—it was that AI assistants were often guessing or using outdated training data instead of checking docs.getdbt.com in real time.

We also looked at how other platforms solve this. Google's approach with their Docs API was particularly inspiring—they recognized that having a massive docs corpus is only valuable if AI assistants can access it in real time. Rather than building yet another API endpoint, they integrated docs fetching into the tools developers already use.

### Why MCP Over a Custom API
We had two paths:
1. **Build a separate dbt Docs API** — Powerful, but adds another integration point teams have to manage
2. **Add docs fetch to the existing dbt MCP server** — Leverage what's already there, no new infrastructure to maintain

We chose option 2 because:
- **Existing adoption:** Teams already using the dbt MCP server get this for free; no new tool to learn
- **Scalability:** MCP is becoming the standard for AI-tool integration; our work feeds into a broader ecosystem
- **Simplicity:** Fewer APIs to manage means fewer things to break, update, and document
- **User choice:** Whether you're using dbt agent skills, Claude, or another tool—if it supports MCP, it works

Plus, honestly? Google's docs API works because it's part of a unified ecosystem. Our dbt MCP server is becoming that for the dbt community.

---

## How We Validated the Decision (And What We Learned)

[1-2 paragraphs about the research and testing process, including dbt Platform usage]

To validate this approach, we dug into the data. Using dbt Platform's internal analytics and the Insights feature, we tracked:
- **docs.getdbt.com page views over time** — Spoiler: they're decreasing
- **Why they're decreasing** — Users are increasingly consuming docs through AI assistants instead of browsing directly
- **dbt MCP server usage patterns** — How often teams query it, what they're asking, where it's being used

The insight? Users are already voting with their behavior. They prefer getting answers *in context* (via AI + MCP) over manually navigating to docs. Page view decline isn't a problem—it's validation that we're solving the right problem.

**Fun fact:** As a technical writer, I used dbt Platform's Insights feature to analyze this data myself. Building queries to understand how users interact with docs is pretty meta—and it's exactly why tools like dbt matter. You can turn raw usage patterns into actionable intelligence. It's the same value proposition we're enabling for you with the MCP server + docs fetch combo: get the answers you need, where you're already working.

Testing against dbt Platform, dbt Cloud, and dbt Core confirmed the approach works across deployment models. The feature felt natural—like it should have always been there.

---

## The Problem: Documentation Drift Across Your Team

[2-3 paragraphs]
- **Developers:** Writing a macro, wondering about Jinja syntax or dbt-specific functions
- **Analysts:** Exploring a model, asking "What does this do?" or "Is it safe to use?"
- **Managers:** Reviewing data lineage, needing quick answers about model dependencies
- **The shared friction:** Leaving their current context (editor, notebook, Slack, AI chat) to search docs.getdbt.com
- **The risk:** Outdated answers, incomplete context, or relying on guesses instead of the source of truth
- **The cost:** Broken flow state, slower iteration, decisions made on incomplete information

---

## The Solution: Docs Fetch in the MCP Server

[2-3 paragraphs explaining what was added and why it matters]

### What we added
- The dbt MCP server can now fetch and serve relevant documentation from docs.getdbt.com on demand
- When you ask Claude (or another AI assistant using the dbt MCP server) a question about dbt, it can pull the authoritative docs in real time
- Works whether you're using the MCP server **locally** (in VS Code, your terminal, your IDE) or **remotely** (querying a shared dbt project across your team)

### Why this is powerful
- **Accuracy:** Docs are always up-to-date; AI isn't guessing from stale training data
- **Completeness:** Get the full context from official docs, not a paraphrase
- **Speed:** No manual searches; the AI does the lookup and synthesizes the answer for your specific question
- **Confidence:** You know the answer comes from the source of truth, not hallucination
- **Team alignment:** Whether you're local or remote, everyone gets the same authoritative answers

---

## Real-World Scenarios: Before & After

### Scenario 1: Developer Writing a Macro (Local Use)

#### Before
```
1. Writing dbt macro in VS Code
2. "Wait, what's the exact syntax for dbt_utils.generate_series?"
3. Alt-tab to browser
4. Search docs.getdbt.com
5. Read docs
6. Back to editor
7. Resume macro (context lost, momentum broken)
```

#### After
```
1. Writing dbt macro in VS Code
2. Ask Claude directly in VS Code extension
3. Claude fetches docs.getdbt.com and responds with exact syntax + examples
4. Keep coding
5. No context switch, flow state intact
```

---

### Scenario 2: Analyst Exploring Data (Remote Use)

#### Before
```
1. Reviewing a dbt project in dbt platform / dbt Cloud
2. "What does this model actually do? Is it production-ready?"
3. Click to docs or read YAML comments (if they exist)
4. Still unclear; search docs.getdbt.com manually
5. Come back and make a decision based on incomplete info
```

#### After
```
1. In Claude (or your AI chat), ask about the model
2. AI queries the remote dbt MCP server for context + docs
3. Gets a clear explanation of what the model does, dependencies, and usage
4. Make informed decisions without leaving your conversation
```

---

### Scenario 3: Manager Reviewing Lineage (Team Use)

#### Before
```
1. "Why did this test fail? What models depend on it?"
2. Navigate dbt UI, follow lineage manually
3. Ask team member or search Slack history
4. Piece together context from multiple sources
```

#### After
```
1. Ask Claude about the failing test
2. AI pulls context from dbt MCP server + docs
3. Get a complete picture: what failed, why, what depends on it, next steps
```

---

## How We Tested It: dbt Platform, dbt Cloud & dbt Core

### Testing Across Setups
[2-3 paragraphs]
- We validated this during development using dbt platform accounts, dbt Cloud, and dbt Core
- Real benefit: Testing against different deployment models and the dbt Fusion Engine showed where docs fetch shines—when teams are actively building and exploring together
- Mention: dbt platform's intelligent project parsing and Fusion Engine integration made testing faster and validated the feature works in modern, complex dbt setups

### Ensuring Broad Compatibility
[1 paragraph]
- Also validated with dbt Core and traditional deployments to ensure the feature works for the full breadth of dbt users
- Whether you're on dbt Cloud, running dbt Core locally, or using dbt platform, this feature is built for you

---

## Who Benefits? (Use Cases by Persona)

[Bulleted overview, can expand into sections if needed]

**Analytics Engineers & Developers:**
- Write models, tests, and macros faster without manual doc searches
- Get accurate syntax help and best practices in real time

**Data Analysts:**
- Understand what models do and how they're built without diving into code
- Explore lineage and dependencies conversationally
- Make safer decisions about which data to use

**Analytics Managers:**
- Answer team questions about data lineage and model ownership
- Troubleshoot issues faster with AI-assisted context
- Understand your dbt project's structure without being a dbt expert

**Data Governance Teams:**
- Verify documentation accuracy and completeness
- Ensure lineage is correct and well-understood
- Answer compliance questions about data origins and transformations

---

## Practical Examples

### Example 1: Developer Debugging a Test Failure

```
Developer: "Why is my test_not_null failing on the orders model?"

Claude (with docs fetch): "The test_not_null macro checks that a column 
has no null values. Looking at your dbt project, the 'orders' model's 
'order_id' column is failing. This typically means either:
1. Your upstream source has nulls
2. Your dbt seed/source definition allows nulls
See ref() semantics here [docs link] and check your source definition..."
```

### Example 2: Analyst Exploring an Unfamiliar Model

```
Analyst: "What does the fact_sales model do? Can I use it for my report?"

Claude (with docs fetch): "The fact_sales model is a fact table that 
combines order, customer, and product data at the daily level. It's 
marked as mature and used by 5 downstream models. Here's what it includes..."
```

### Example 3: Manager Triaging a Data Issue

```
Manager: "This dbt job failed last night. What broke?"

Claude (with docs fetch): "Looking at your dbt project and the error logs, 
the stg_customers model failed during ref() resolution. This means the 
upstream source it depends on is unavailable. Here's what depends on this 
model downstream [list], and here's how to fix it [guide]..."
```

---

## What This Means for Your Workflow

[Bulleted summary of benefits]
- ✅ Stay in your current context (editor, chat, notebook) longer
- ✅ Get authoritative answers without manual searches
- ✅ Reduce decision paralysis (docs are the source of truth, not memory or guesses)
- ✅ Iterate faster on models, tests, and analysis
- ✅ Works across dbt Cloud, dbt platform, dbt Core, and hybrid setups
- ✅ Works whether you're local or querying a remote dbt project
- ✅ Entire team gets consistent, accurate information

---

## How to Get Started

[Short CTA with different paths for different users]

**Local Users (Developers & Individual Contributors):**
- Install the dbt VS Code extension (or use Claude with dbt MCP server configured locally)
- Ask Claude a dbt question—it will now pull live docs for you

**Remote Users (Teams & Analysts):**
- Use Claude to query your team's dbt project (via the remote dbt MCP server)
- Ask about models, tests, lineage, or dbt concepts
- Docs will be pulled automatically for context

**All Users:**
- Try it on a real project and see how it changes your workflow
- Share feedback: [link to feedback/discussion channel]

---

## Looking Ahead

[1-2 paragraphs on future vision]
- This is the beginning of better AI-assisted analytics engineering across your team
- Docs fetch is the foundation; we're exploring what else the MCP server can do to support developers, analysts, and teams
- Feedback welcome: How are you using the dbt MCP server? What else would help you stay in flow?

---

## Closing

[2-3 sentences bringing it back to the bigger picture]
- dbt is about enabling analytics engineers, developers, and analysts to build and understand better, faster
- Removing friction in the development and exploration loop is core to that mission
- Docs fetch in the MCP server is one more step toward that goal—for everyone using dbt

---

## Metadata / Blog Details
- **Audience:** dbt developers, analysts, analytics engineers, teams using dbt (any deployment model)
- **Tone:** Technical but approachable; practical and action-focused; inclusive of multiple personas
- **Length estimate:** 2000–2500 words (medium-length blog post)
- **Call-to-action:** Try it + share feedback (with different entry points for different users)
- **SEO keywords:** dbt docs, MCP server, dbt Cloud, dbt Platform, Claude, AI-assisted development, analytics, remote dbt, collaborative dbt
