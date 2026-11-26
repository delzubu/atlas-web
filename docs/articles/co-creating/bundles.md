---
title: "Organising complex documents into bundles"
parent: Articles
nav_order: 5
---

# Organising complex documents into bundles

[Co-creating with AI]({{ site.baseurl }}{% link docs/articles/index.md %}) · Part 4 of 6

A useful report can become difficult to maintain as it grows. Its main narrative, evidence tables, illustrations and working notes may be easier to edit separately, but readers should not have to reconstruct the document by exploring a large directory.

A bundle lets a folder behave like one document in the Workspace navigator while retaining the files inside it.

Because a bundle is made of ordinary workspace files, its structure survives a change of agent or provider. A second agent can inspect the same main page and supporting material, rather than reconstructing the document from the first agent's conversation.

## A document can be more than one file

For the exhibition, we might organise the market-square dossier as follows:

```text
working/
  market-square/
    overview.md
    chronology.md
    commercial-life.md
    civic-life.md
    evidence.md
    images/
      square-sketch.svg
```

The overview explains what the dossier covers and links to the other sections. The separate files allow an agent to revise the chronology without rewriting the account of civic life or disturbing its review comments.

A bundle is not a ZIP archive. Its files remain ordinary workspace files, accessible to people and agents, with their individual links and revision history.

## Make the folder into a bundle

In Workspace, open the folder's menu or right-click the folder and choose **Make into a bundle…**.

1. Select `overview.md` as the main page, or choose **None** if you want the bundle to open on its contents list.
2. Enable **Show as a single item in the file tree** if you want to hide the expanded folder from the normal navigator.
3. Enable **Show the contents panel** so readers can move between its files from the opened document.
4. Select **Make bundle**.

Opening the bundle now leads to its main page. The contents panel provides access to the supporting files. Links directly to a section still work. Use **Bundle settings…** to adjust the presentation, or **Make a plain folder again** to remove the bundle settings without removing its contents.

## The same idea as an imported document, with different ownership

![Two ways to present a collection of files as one artifact]({{ site.baseurl }}{% link assets/images/articles/intake-and-bundles.svg %})

An imported PDF or PowerPoint file hides the generated extraction artifacts behind the source's interface. A bundle uses a similar presentation principle for files you organise yourself.

There is an important difference: extracted fragments are derived from an original source and may be regenerated. Bundle files are independently authored content. Editing a bundle's chronology is a change to the dossier itself, not a change to a generated preview of another document.

## Work on a bundle with an agent

You do not need a special prompt language. Ask the agent to keep the main page and supporting files coherent:

> Read the market-square bundle's main page and evidence table. Update chronology.md using the reviewed dates, then update the overview only where its summary is affected. Read the open comments in both files, preserve unrelated sections, and report the exact changes. Do not move the original sources out of inputs/.

This allows the agent to work at the right granularity. It can read the overview to understand the whole dossier, then focus on the specific file that needs attention.

When adding a new section, ask the agent to update the main page or its links if needed. Use a direct link to a section when asking someone to review it, and the bundle link when sharing the complete artifact.

## Decide when to use bundles

A bundle works well when related files have a shared identity: a proposal, a research dossier, a case study or a collection of slides. It is less useful for an ordinary inbox or a directory of unrelated reference files.

Keep independent source documents outside an authored bundle when that makes provenance clearer. Avoid hiding folders merely for tidiness if people frequently need to work across them. Nested bundles are possible, but add structure only where readers benefit from it.

Specialised Atlas documents, such as plans, already have their own document identity and presentation. Use their own app and avoid putting a bundle marker inside their internal document folders.

Our exhibition dossier now has a human-friendly entry point and manageable sections for agents to edit. The next article shows how to make its knowledge and presentation more expressive than prose alone.

See [Group related files into a bundle]({{ site.baseurl }}{% link docs/apps/workspace.md %}#group-related-files-into-a-bundle) for the Workspace controls.

---

[Previous: Working with documents beyond Markdown]({{ site.baseurl }}{% link docs/articles/co-creating/working-with-documents.md %}) · [Next: Going beyond documents]({{ site.baseurl }}{% link docs/articles/co-creating/beyond-documents.md %})
