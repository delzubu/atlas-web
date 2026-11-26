---
title: "From raw material to finished work"
parent: Articles
nav_order: 7
---

# From raw material to finished work

[Co-creating with AI]({{ site.baseurl }}{% link docs/articles/index.md %}) · Part 6 of 6

Suppose you have been asked to prepare a small exhibition on how a town's market square changed over time. You receive a few documents and a broad brief. You do not yet know the final story, and the available evidence may contradict your initial expectations.

This is a good example of co-creation: the work evolves as you understand the problem. You and an agent investigate it together, build an explicit representation of what you learn, review it and finally communicate the result.

For the general working method, including separate report, research and model folders and cross-domain consolidation, read [A practical workflow for co-creation]({{ site.baseurl }}{% link docs/articles/structured-co-creation.md %}). This article illustrates the approach with a local-history example.

The town and example evidence below are fictional. Replace them with your own sources when following the workflow.

![From inputs through research and modelling to reviewed output]({{ site.baseurl }}{% link assets/images/articles/research-to-output.svg %})

You do not need to use one agent for the whole process. For example, use the agent you prefer for interpreting sources, another for checking the knowledge model, and a third for preparing the presentation. Each receives the relevant project context and works with the same persistent artifacts. The choice of model or subscription can change as the work develops; the project's purpose and history remain in Atlas. Delegate bounded work autonomously where useful, and intervene at the points where your expertise changes the outcome.

## 1. Capture the inputs and the goal

Create a workspace, a short README.md, working instructions and a handoff. Import the source documents into `inputs/`. Set a concrete audience and outcome: a short public-facing historical narrative, an illustrated chronology and a presentation for the exhibition organisers.

You might start with this request:

> Read the project instructions and exhibition brief. Inventory the supplied sources. Identify the goal, constraints, relevant facts, assumptions and unresolved questions. Save your interpretation in working/exhibition-brief.md and research/source-inventory.md. Do not start writing the exhibition text yet.

Review the brief yourself. This is the first opportunity to correct the agent's understanding before it commits to an unsuitable approach.

## 2. Interpret the available material

Ask the agent to produce a source-backed evidence table. The following rows illustrate the kind of result you want; the source names and dates are examples only.

| Date | Evidence recorded | What remains uncertain |
| --- | --- | --- |
| 1842 | A market register lists stalls in the square | Whether stalls were present throughout the year |
| 1875 | A town plan shows a new civic building at the square | Whether construction displaced traders |
| 1908 | A photograph shows both market stalls and civic activity | How representative that particular day was |

Each real row should link to the actual source and, where possible, the relevant page or section. Preserve the distinction between a source's statement, an interpretation of it and what you cannot establish.

Notice how the illustrative 1908 evidence challenges the initial idea that commerce simply gave way to civic activity. The developing understanding is more important than defending the first proposed narrative.

## 3. Investigate the gaps

Review the evidence and add comments where further research is needed. Ask the agent to investigate specific questions rather than collect an unlimited amount of material:

> Investigate whether market activity and civic use coexisted after the new building opened. Use the available library or permitted external research tools. Save relevant references and findings in research/market-and-civic-use.md. Distinguish primary evidence from later interpretations. Report contradictory evidence and inaccessible sources.

An agent with Search access may query an indexed historical library; another may locate material through KB or use its own research tools. Atlas does not guarantee access to external sources. If a source cannot be verified, record the limitation and decide whether it matters.

Update the evidence table and handoff. At this point you should have an improved understanding, not merely more notes.

## 4. Model what you have learned

A model makes selected aspects of the subject explicit so you can inspect them, relate them and use them again.

For the exhibition, an appropriate model is a KB containing event and place articles. Define the article types, timeframes and relationship vocabulary first, then ask the agent to build the model:

> From the reviewed evidence, create or update the exhibition knowledge base with articles for the square, the market activity and the documented civic developments. Add timeframes and explicit, supported relationships. Cite the sources and distinguish uncertain interpretations. Do not infer that one event caused another merely because it happened earlier.

The resulting Timeline makes the sequence visible; the Graph lets you inspect declared relationships. Article text explains the evidence and uncertainties behind them. Keep the source inventory and detailed research in the workspace.

A different project might require a causal model, UML diagram, plan or map. Choose the model based on the question, not on the number of available apps.

## 5. Review the model before rendering

Review the article content and relationships against the sources. Record review comments on the working dossier or review notes linked to the relevant KB articles; use the relevant app's editing workflow for corrections.

Try asking a second agent for an independent review:

> Review the exhibition model against the cited evidence. Identify missing support, inconsistent dates and relationships that are stronger than the evidence permits. Record findings in working/model-review.md and comment on relevant passages of the working dossier. Do not silently revise uncertain historical conclusions.

After reviewing the findings, ask for targeted corrections and replies to the comments. Leave unresolved matters visible. If a claim cannot be supported, narrow it or remove it from the intended public narrative.

You may need to return to research or even revise the original brief. This is a normal part of the workflow, not a failure to follow a plan.

## 6. Render the reviewed model for its audience

Only now ask the agent to write the exhibition text and create a presentation:

> Use the reviewed KB articles and evidence table as the basis for output/exhibition-text.md and a short static HTML slideshow in output/exhibition-preview/. Write for local residents and school groups, explain the changing uses of the square, and distinguish documented conclusions from uncertainty. Keep links to the supporting articles and sources in the working material. Do not introduce new historical claims during presentation writing.

The report and slides are authored views of the reviewed model. Rendering here means turning structured understanding into an appropriate audience-facing artifact; it is not a claim that Atlas automatically generates every output from a KB.

Inspect the output in Workspace, comment on the wording or layout and ask for revisions. If a review identifies a factual error, correct the underlying model and update the affected outputs rather than fixing just one slide.

## 7. Leave the project ready for further work

The final workspace might look like this (specialised Atlas documents also have their own managed folders):

```text
market-square-exhibition/
  README.md
  AGENTS.md
  HANDOFF.md
  inputs/
  research/
    source-inventory.md
    evidence.md
    market-and-civic-use.md
  working/
    exhibition-brief.md
    market-square/
    model-review.md
  output/
    exhibition-text.md
    exhibition-preview/
```

The KB and optional preparation plan can be opened through their respective apps or the project overview. Update the handoff with the reviewed deliverables, outstanding questions and what needs to happen next. The sources, research, model and outputs remain accessible even when the current conversation ends.

## What makes this co-creation?

You did not ask the agent to produce an exhibition from an incomplete brief in one pass. You delegated meaningful work, inspected intermediate results, challenged interpretations, revised the model and shaped the final presentation.

The same principle applies to a technical design, a research report, a proposal or a fictional world. Use a persistent workspace, choose a suitable representation, review the work where it lives and let the outcome evolve as your understanding improves.

Atlas supports that method through shared artifacts and specialised tools. You decide how much structure and review the task warrants. For a short note, most of these stages may be unnecessary; for substantial research, they help keep the result traceable and maintainable.

Explore the [app guides]({{ site.baseurl }}{% link docs/apps/index.md %}) for the individual tools, and the [playbook library]({{ site.baseurl }}{% link docs/playbooks/index.md %}) for workflows you can delegate.

---

[Previous: Going beyond documents]({{ site.baseurl }}{% link docs/articles/co-creating/beyond-documents.md %}) · [Return to the series]({{ site.baseurl }}{% link docs/articles/index.md %})
