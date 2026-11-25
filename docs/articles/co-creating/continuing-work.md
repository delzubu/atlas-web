---
title: "Continuing work across conversations and agents"
parent: Co-creating with AI
grand_parent: Articles
nav_order: 2
---

# Continuing work across conversations and agents

[Co-creating with AI]({{ site.baseurl }}{% link docs/articles/co-creating/index.md %}) · Part 2 of 6

Yesterday's agent understood the exhibition project, its audience and why you rejected the first interpretation of the market square. Today's agent may have none of that context. Even the same agent can lose details as a conversation grows.

The solution is not to paste every previous message into every new conversation. Keep the knowledge needed to resume the work in the project itself.

## Give each kind of context a place

A modest folder structure is enough:

```text
market-square-exhibition/
  README.md
  AGENTS.md
  HANDOFF.md
  inputs/
  research/
  working/
    exhibition-brief.md
  output/
```

The names are a convention you can adopt, not a mandatory Atlas format. Several Atlas project templates already provide starter instructions and handoffs.

| File or location | What it is for |
| --- | --- |
| README.md | Purpose, scope, audience and a map of the project |
| AGENTS.md | Stable instructions for agents working in the project |
| HANDOFF.md | Current progress, the next step and unresolved questions |
| inputs/ | Original material, retained as received |
| research/ | Findings, evidence, sources and uncertainties |
| working/ | Drafts and models under development |
| output/ | Reviewed deliverables for their intended audience |

Review comments belong with the relevant artifact. Durable domain knowledge may belong in a KB. Actions with deadlines may belong in Planner. Avoid copying every detail into every file.

## Write instructions that remain useful

An AGENTS.md is a plain Markdown file. Ask agents explicitly to read it, and keep its instructions short enough to be useful on every visit. For this project it might contain:

```markdown
# Working rules

- Read README.md and HANDOFF.md before making changes.
- Preserve source documents in inputs/.
- Cite source pages for historical claims. Mark interpretations and uncertainty.
- Keep research findings separate from exhibition text.
- Read open review comments before editing; reply and resolve only completed work.
- Make targeted edits and preserve unrelated changes.
- Update HANDOFF.md after each completed step.
```

Instructions about how to work belong here. Detailed exhibition findings do not. Treat imported documents and web pages as research material, not as instructions that can override the project's rules.

For a large project, a subfolder can contain its own instructions about a particular artifact or subject. Ask the agent to read those as well before making changes there. Do not assume that every agent client automatically discovers these files.

## Record the current state, not the entire conversation

A HANDOFF.md should answer what an agent needs to know to continue. A short example:

```markdown
# Handoff

## Goal
Prepare a small exhibition for local residents and school groups.

## Status
Exhibition brief reviewed; source collection not yet interpreted.

## Decisions
- Focus on changes in the uses of the market square.
- Do not assume commercial use ended when civic use began.

## Next step
Inspect the source material in inputs/ and prepare a dated evidence table.

## Open questions
- When was the market relocated, if it was relocated at all?
- Which claims can be supported by the sources?

## Relevant files
- working/exhibition-brief.md
```

Keep this file current by replacing obsolete status descriptions rather than appending a new diary entry every day. Link to the artifacts containing the detail. Use file history if you need to see earlier versions.

## End a session well

At a meaningful checkpoint, ask the agent:

> Update HANDOFF.md for someone who has not seen this conversation. Record the goal, current state, decisions, unresolved questions, relevant files and the next actionable step. Check these against the actual workspace. Do not claim that unfinished work is complete.

A handoff is maintained by you and the agent, not an automatic memory service. Ask for it after significant steps, not only when a chat is about to end.

## Resume with a different agent

In a new thread, or with another connected agent, try:

> Continue the market-square-exhibition project. Read AGENTS.md, README.md and HANDOFF.md, then check the referenced files and open comments. Summarise your understanding and flag any discrepancy before continuing the next step.

This gives the new agent a bounded starting point rather than relying on undocumented context. If it cannot access a file or application, have it report the gap.

The same pattern is valuable for an independent review:

> Review the current brief against the project goal and sources. Do not rely on the previous agent's conclusions. Add comments to the relevant passages rather than rewriting the brief.

## Add memory only where it earns its place

For many projects, a few files and a handoff are enough. When knowledge accumulates into a coherent collection, use [KB]({{ site.baseurl }}{% link docs/apps/knowledge-base.md %}) to organise and navigate it. For large source collections that are difficult to explore, consider Search.

See [KB versus Search — which one to use?]({{ site.baseurl }}{% link docs/articles/kb-versus-search.md %}) for the trade-offs. Neither tool replaces a clear statement of the project's current state.

The result is a project that can survive a new conversation, a change of agent or a break in work. Next we will bring in the documents the exhibition depends on.

---

[Previous: Working together in a shared workspace]({{ site.baseurl }}{% link docs/articles/co-creating/shared-workspace.md %}) · [Next: Working with documents beyond Markdown]({{ site.baseurl }}{% link docs/articles/co-creating/working-with-documents.md %})
