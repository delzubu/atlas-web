---
title: "Working together in a shared workspace"
parent: Articles
nav_order: 2
---

# Working together in a shared workspace

[Co-creating with AI]({{ site.baseurl }}{% link docs/articles/index.md %}) · Part 1 of 6

A useful conversation with an agent often ends with a long answer. You copy it into a document, edit it, paste the changed version back into chat and ask for another revision. Before long there are several copies, and neither you nor the agent is certain which one is current.

Workspace gives both of you a common place to keep the work. The conversation is where you discuss it; the workspace is where the work persists.

## Choose the agent, keep the work

Atlas — The Light Factory separates the workspace from the AI platform. You can work with a compatible agent in ChatGPT, Claude or another MCP client, then continue with a different one using the same Atlas files, comments and revision history. For example, delegate a first draft using one subscription, ask another agent to review it independently, and edit the result yourself in the browser. You do not need to export the draft and re-import it just to switch agents.

Provider-native artifacts, such as [Claude Artifacts](https://support.claude.com/en/articles/17153992-what-are-artifacts-and-how-do-i-use-them), also support creating, editing and sharing work. They are useful when working within that platform. Atlas takes a different approach: the persistent project and its review process are independent of the AI service that helped produce them.

Model-agnostic does not mean every client or subscription supports every operation. Your chosen client needs the appropriate MCP capabilities, connection and permissions. Atlas does not transfer conversation history, combine subscription quotas or provide access to models you have not subscribed to.

## Start with a shared artifact

Create a workspace for a local-history exhibition, or use a project you are already working on. Open it in [Workspace]({{ site.baseurl }}{% link docs/apps/workspace.md %}) and connect an agent with write access.

Instead of asking the agent to write a complete exhibition proposal in chat, ask it to create a first working document in the workspace:

> In the exhibition workspace, create `working/exhibition-brief.md`. Outline the intended audience, the story we want to tell and the questions we need to investigate. Mark assumptions explicitly. Do not invent historical findings. Give me a link to the file when it is ready.

Open that file in the navigator. You now have a common artifact to inspect and improve. You can read the rendered Markdown, switch to the source editor when useful and save your own changes.

## Edit directly or leave a comment?

Use direct editing for a change you already know how to make. For instance, replace “school groups” with “local residents and school groups” if that is the audience you want.

Use a comment when something needs interpretation, investigation or discussion. Select the relevant passage and add a review comment. Consider this first draft:

> The exhibition will explain how the market square developed from a trading centre into a civic gathering place.

A useful comment would be:

> This assumes there was a clear transition. Check whether the two uses coexisted. If the sources cannot establish a change, frame it as a research question rather than a conclusion.

The comment identifies the problem, states what needs to be checked and explains what a satisfactory change would look like. It is much easier for an agent to act on than “improve this paragraph”.

## Ask the agent to work through comments

Save your edits and comments, then ask:

> Read the current exhibition brief and its open review comments. Make targeted changes where the requested correction is clear. Reply to each comment explaining what you changed, and resolve only the threads you have actually addressed. Leave unresolved questions open and explain what is missing. Preserve my edits and unrelated material.

Atlas supports review threads on Markdown and HTML pages, and on supported objects in plans and maps. Agents can read, reply to, resolve and reopen threads. A resolved thread records that the discussion was dealt with; it is not proof that its conclusion is correct.

For the example above, a suitable response might replace the unsupported assertion with a question about changing uses of the square, reply with the reason and resolve that particular wording issue. A separate comment requesting evidence that has not yet been found should remain open.

You can inspect the reply and the revised text, then reopen the discussion by replying if further work is needed.

![The co-creation cycle]({{ site.baseurl }}{% link assets/images/articles/co-create-cycle.svg %})

## Inspect changes instead of regenerating everything

Use file history or the project's **Changes** view to see what changed. A small correction should normally produce a small revision, not a replacement of the whole document.

If the agent misunderstood a requirement, identify the specific change to reverse or amend. Do not discard useful work simply because one paragraph is wrong. Workspace history also provides a way to recover previous material, subject to your access and the nature of the change.

Avoid having two editors make overlapping changes at the same time. Save before handing control to the agent, and reopen the current version before editing again. If a revision conflict occurs, inspect the competing changes instead of forcing an overwrite.

## Make review a regular part of the work

A productive session might consist of ten minutes of discussion, a draft, a few direct edits, three comments and a targeted revision. The exact order can vary. The important thing is that both participants continue working on the same saved artifact.

For the exhibition, the first result is a reviewed brief and a set of open research questions, not a polished exhibition. In the next article we will preserve enough project context for another agent to continue without the original conversation.

See the [Workspace guide]({{ site.baseurl }}{% link docs/apps/workspace.md %}) and [Assistant guide]({{ site.baseurl }}{% link docs/apps/assistant.md %}) for the application controls.

---

[Series overview]({{ site.baseurl }}{% link docs/articles/index.md %}) · [Next: Continuing work across conversations and agents]({{ site.baseurl }}{% link docs/articles/co-creating/continuing-work.md %})
