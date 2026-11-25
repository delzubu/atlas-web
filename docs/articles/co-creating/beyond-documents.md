---
title: "Going beyond documents"
parent: Co-creating with AI
grand_parent: Articles
nav_order: 5
---

# Going beyond documents

[Co-creating with AI]({{ site.baseurl }}{% link docs/articles/co-creating/index.md %}) · Part 5 of 6

A well-organised document is an excellent starting point. But a narrative is not always the best way to explore dates, relationships, tasks or a presentation for an audience. Atlas lets you keep those representations alongside the source material and working notes rather than forcing everything into one long text.

The aim is not to add visual polish for its own sake. Choose a representation that helps you understand or communicate something more effectively.

## Give the audience a slideshow

For the exhibition, a short presentation could introduce the square, show its changing uses and end with the questions that remain unresolved.

Workspace can present HTML pages as a slideshow. Ask an agent to create a small deck as separate static HTML files in a dedicated folder:

```text
output/
  exhibition-preview/
    01-introduction.html
    02-market-life.html
    03-civic-life.html
    04-open-questions.html
    images/
```

Open an HTML slide in Workspace and use the slideshow view. Pages in the folder and its visible subfolders follow natural filename order, so numbered filenames make the intended sequence clear. Keep unrelated HTML pages outside the slide folder.

An example request:

> Create four static HTML slides for a five-minute exhibition preview. Use a consistent 16:9 layout, accessible text and local images. Use stable element IDs for slides, charts and figures so comments stay attached when you revise them. Base every historical claim on the reviewed dossier and cite its source. Leave uncertain dates labelled as uncertain.

Workspace's HTML preview does not run scripts or load external web resources. Keep images and styles in the workspace or inline. HTML slides are editable source artifacts; a native PowerPoint export is a separate task, not a promise of the slideshow feature.

Inspect the slides, leave comments on the text or figures and ask the agent to revise them. You can review the presentation with the same collaboration pattern used for Markdown.

## Make knowledge navigable

If the exhibition grows beyond a single dossier, create a [knowledge base]({{ site.baseurl }}{% link docs/apps/knowledge-base.md %}) for the places, people and events you want to maintain.

Ask the agent to agree with you on article types, tags, timeframes and meaningful relationship names. For example, articles for a market charter, a road alteration and the opening of a civic building may be connected by explicit relationships. Use KB tools to write and update formal KB articles so their indexes remain current.

Browse by date in Timeline, by subject in Files or by declared relationships in Graph. A normal Markdown folder can also be inspected as a graph view without becoming a formal KB.

Do not invent causal relationships just to make the graph more connected. A dated event preceding another does not necessarily cause it. Mark uncertain relationships and preserve the evidence supporting them.

## Plan work rather than listing dates in a paragraph

A list of exhibition preparation tasks may be adequate at first. Once layout, permissions, printing and installation depend on one another, a [Planner]({{ site.baseurl }}{% link docs/apps/planner.md %}) plan is more useful.

> Create an exhibition preparation plan. Work backwards from the opening date, show dependencies between source review, text approval, design, printing and installation. Mark assumptions about lead times and leave resource assignments open where I have not supplied them.

Inspect the Gantt chart and change tasks or dependencies as you learn more. Comments on plan items allow you to discuss a specific task. Saved plans can also contribute TODOs to the workspace's project overview.

Keep historical dates in the KB timeline and delivery deadlines in the preparation plan. They answer different questions even though both use time axes.

## Use a model when the question needs one

| If you need to… | Consider… |
| --- | --- |
| Explain an argument or tell a story | Markdown or HTML |
| Organise related facts and events | KB |
| Communicate a sequence of messages | HTML slideshow |
| Schedule activities and dependencies | Planner |
| Explore causes and feedback | Causal |
| Describe components and interactions | UML |
| Represent a physical layout | Map or Mapper |

The specialised apps available to you depend on your instance and access. Use the simplest representation that helps with the question; there is no reason to create a causal model or a spatial model if the exhibition does not need one.

Models should retain a clear connection to their sources and assumptions. A diagram is not stronger evidence than the material from which it was built.

## Keep the working material and the presentation separate

The exhibition dossier, knowledge articles and preparation plan can coexist with a presentation. Update the reviewed source of a claim first, then revise the presentation that communicates it. Do not assume that independently authored slides or reports automatically refresh when a model changes.

Our project now has several ways to work with the same subject: read the sources, navigate structured knowledge, inspect a plan and review a presentation. The final article puts these pieces into an end-to-end workflow.

See [Workspace]({{ site.baseurl }}{% link docs/apps/workspace.md %}), [KB]({{ site.baseurl }}{% link docs/apps/knowledge-base.md %}), [Planner]({{ site.baseurl }}{% link docs/apps/planner.md %}) and the [presentation playbooks]({{ site.baseurl }}{% link docs/playbooks/work.md %}#presentations) for further guidance.

---

[Previous: Organising complex documents into bundles]({{ site.baseurl }}{% link docs/articles/co-creating/bundles.md %}) · [Next: From raw material to finished work]({{ site.baseurl }}{% link docs/articles/co-creating/from-inputs-to-output.md %})
