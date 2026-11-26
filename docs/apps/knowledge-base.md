---
title: Knowledge base
parent: Core
grand_parent: Apps
nav_order: 3
---

# Knowledge base

Use Knowledge base (KB) to organise articles with fields such as timeframe, tags, and location. Browse them in a file tree, filter a timeline, or explore relationships between articles in a graph. A knowledge base can live in an Atlas workspace or a GitHub or GitLab repository.

## How to use Knowledge base

1. In KB Explorer, choose **Open knowledge base…** and select a workspace or Git repository. With write access, you can create an empty knowledge base in a workspace.
2. Browse **Files** for the articles, **Timeline** for dated material, or **Graph** for relationships. Use filters such as tags, location and timeframe.
3. Create or edit an article, then open **Review & commit** to apply staged edits. Add a timeframe or tags when you want an article to be discoverable through those filters.

Workspace knowledge bases can also register Tech Radar and Expectations curve views for assessing technologies or products. A plain workspace folder can be opened as a graph for inspection.

## See the knowledge you are working with

![KB Explorer with article navigation and highlights]({{ site.baseurl }}{% link docs/developers/design/screenshots/kb-files-highlights.png %})

*Reference screenshot from the design system.*

For a first project, ask an agent to establish a small KB about a subject you know well. Agree on titles, tags and article types, create a handful of sourced articles, then navigate them in Files, Timeline or Graph as appropriate. Review and correct the articles rather than relying solely on chat answers.

KB is intended for lightweight structured knowledge; semantic retrieval over large document collections is a separate optional capability. See [KB versus Search]({{ site.baseurl }}{% link docs/articles/kb-versus-search.md %}).

## Playbooks supporting work

[Set up a knowledge base]({{ site.baseurl }}{% link docs/playbooks/core.md %}#setup-knowledge-base) defines conventions and seeds the first articles. [Model relations]({{ site.baseurl }}{% link docs/playbooks/hobby.md %}#model-relations) helps turn dossiers into a graph of related people, places and organisations.
