---
title: Your first shared project
parent: Getting started
nav_order: 1
---

# Create and review your first shared artifact

This small exercise demonstrates the core Atlas experience: a person and an agent editing and reviewing the same persistent artifact. You need an Atlas instance, Workspace writer access and a compatible connected agent. If you are evaluating the product, you can inspect the example files below without an account.

[Inspect the example files](https://github.com/delzubu/atlas-web/blob/main/assets/examples/first-project/README.md). The before and after drafts and review note are illustrative, not exported application activity.

## 1. Create the project

In Workspace, create an empty project called something like `first-co-creation`. Use a different name if that already exists. Open the project overview.

Ask your agent:

> In my first-co-creation workspace, create AGENTS.md describing this exercise: prepare a brief for a community workshop, distinguish facts from assumptions, and retain open questions. Then create brief.md with a short draft. We have a room for 20 people; the date and budget are not yet agreed.

Check that the agent returns a file reference, not just the draft in chat.

## 2. Inspect the draft

Open brief.md in Workspace. An example starting point is:

```markdown
# Community workshop

## Goal
Introduce participants to the proposed community project.

## Constraints
- Room capacity: 20 people.
- Date: not agreed.
- Budget: not agreed.

## Proposed format
A short introduction followed by group discussion.
```

If the agent invents a date or budget, correct it before proceeding.

## 3. Review and refine

Select the proposed format in the document and add a review comment:

> Make this a working session, not an introductory event. Participants should leave with two agreed next actions. Keep the unknown date and budget visible.

Save your comment. Ask the agent:

> Read brief.md and its open review comments. Revise the proposed format, preserve the constraints, reply to the comment with what changed, and resolve it only if addressed.

Inspect the current file and the thread. A satisfactory change might replace the format with a short introduction, facilitated discussion and agreement on two actions. The date and budget remain unknown.

![Reference screenshot of a document review header in Atlas]({{ site.baseurl }}{% link docs/developers/design/screenshots/kb-review-header.png %})

*Reference image from the design system showing a review surface; not a screenshot of this example project.*

## 4. Continue with another agent

If you have another compatible connected AI client, open a new conversation there and ask:

> Read AGENTS.md and brief.md in first-co-creation, including the review discussion. Check whether the revised brief preserves the constraints. Report any remaining issues without changing the file.

This agent does not need the original conversation. It needs access to the project and sufficient context to do its task.

## 5. Inspect the history

Open the project's Changes view or file history. Check the original and revised contents. You can make a further manual edit, leave another comment or ask the agent to update the brief.

You have now used the basic co-creation loop without needing an external database, a specialised model or a long project setup.

For more detail, see [Workspace]({{ site.baseurl }}{% link docs/apps/workspace.md %}) and [Working together in a shared workspace]({{ site.baseurl }}{% link docs/articles/co-creating/shared-workspace.md %}). For a larger project, use [the structured co-creation workflow]({{ site.baseurl }}{% link docs/articles/structured-co-creation.md %}).
