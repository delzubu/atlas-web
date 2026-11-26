---
title: "Creating a technical presentation with AI"
parent: Articles
nav_order: 10
description: "A worked example of turning a presentation brief into researched messages, a reviewed storyline and rendered slides."
---

# Creating a technical presentation with AI

A presentation is more than a set of attractive slides. Before you decide how a slide looks, you need to know what the audience should understand, what evidence supports the message and how the individual messages fit together.

This worked example applies [A practical workflow for co-creation]({{ site.baseurl }}{% link docs/articles/structured-co-creation.md %}) to a technical presentation about Atlas itself. The example audience and slot are illustrative, not a record of a real meeting.

## Inputs: decide what the presentation is for

Suppose you have 20 minutes with solution architects. Your intent is to explain how Atlas supports expert-led knowledge work, how it connects agents to shared artifacts, and where specialised tools add value. You want the audience to understand why Atlas is useful even if they already use an AI subscription.

Create a workspace from a suitable template or from scratch. Record that goal in AGENTS.md. In `inputs/`, retain your briefing notes, any invitation or agenda, presentation constraints, supplied examples and relevant visual templates.

If the event organiser has not specified the audience or format, record the missing information instead of guessing.

## Report: establish a common understanding

Ask the agent to summarise the supplied inputs as linked OKF-style articles in `report/`. For this example, useful topics would be the audience, the intended message, the time limit and the desired outcome.

Inspect the interpretation. Does “technical presentation” mean an architectural deep dive, a product demonstration or a discussion of agentic workflows? These are different deliverables. Resolve the distinction before investing in research.

## Research: investigate the product and the audience

Ask the agent to evaluate the existing Atlas documentation, including the product brand book, application guides, specifications and published articles. Where relevant, check current implementation evidence rather than assuming that an aspirational product description is already a shipped capability.

Save findings in `research/` with source links and revision or publication information where relevant. Useful articles might cover:

- The Light Factory positioning and its distinction from repeatable, specification-led automation.
- Workspace, review comments and persistent artifacts.
- Model-agnostic collaboration through compatible MCP clients and existing AI subscriptions.
- KB, Search and specialised representations, including their limitations.
- The audience's professional interests, technical responsibilities and documented communication preferences.

For audience research, use relevant material you are entitled to access: previous meeting notes, supplied correspondence or Atlas Stakeholders records if available. Keep sensitive audience notes in the project. Do not invent personal profiles or infer unsupported preferences.

At this point, do not draft the storyline or decide the slide order. Research establishes what is known and what remains uncertain.

For example, the brand book supports describing Atlas as a platform for bespoke work with evolving requirements. The application documentation supports describing shared Workspace artifacts and review. Neither establishes a measured productivity improvement unless separate evidence exists. Keep that distinction in the research.

## Model: develop the argument before designing slides

The presentation model is the structure of the communication, not the final presentation.

Organise it into manageable sub-domains. A possible layout is:

```text
model/
  audience.md
  key-messages.md
  supporting-evidence.md
  storyline.md
  slides-brief.md
  review.md
```

Where installed, the [Work communication and presentation playbooks]({{ site.baseurl }}{% link docs/playbooks/work.md %}#presentations) provide useful methods. The audience-tailoring playbook can help fit the message to the room; the storyline playbook develops the governing thought, action-title sequence and one message per slide. Let the agent use the actual playbook's required artifact paths, or agree on a compatible layout rather than changing its conventions silently. Some shipped playbooks use `work/` and `slides/` rather than `model/`.

### Tailor the message

Use [Tailor a message to an audience]({{ site.baseurl }}{% link docs/playbooks/work.md %}#tailor-message-to-audience) if available. Give the agent the reviewed research, audience and desired outcome.

The audience model should distinguish what is known from what is assumed. Solution architects may be interested in interoperability, security boundaries and extensibility; that is a reasonable starting hypothesis, not a confirmed preference of every attendee.

A possible governing message is:

> Atlas separates persistent knowledge work from the choice of AI agent, allowing experts to explore, delegate and refine complex artifacts across compatible clients.

This is a proposed synthesis of the researched product positioning, not a quotation from the research.

### Define the story arc

Use [Plan a deck storyline]({{ site.baseurl }}{% link docs/playbooks/work.md %}#plan-deck-storyline) to turn the message into an argument suitable for the slot. Review the sequence before asking for slide designs.

For example:

| Slide | Action title | Supporting material |
| --- | --- | --- |
| 1 | Not every AI task begins with a finished blueprint | Light Factory positioning and a concrete example |
| 2 | Persistent artifacts turn conversations into cumulative work | Workspace and review workflow |
| 3 | Choose the agent without moving the project | Compatible MCP clients, handoffs and independent review |
| 4 | Use the representation that fits the problem | KB, Planner, models and maps |
| 5 | Separate research, modelling and presentation | The staged co-creation method |
| 6 | Start with one shared artifact and refine it together | A short demonstration and next step |

The titles should read as a coherent argument without the slide bodies. Each slide brief should identify its intended message, supporting evidence, role in the narrative and any visual idea that helps convey it. Missing evidence stays visible.

### Consolidate the sub-models

Review the audience model, key messages, evidence and storyline together. If the audience wants technical depth but the storyline promises only generic benefits, revise the message and slide sequence. If a slide implies universal client compatibility but the evidence supports only compatible clients with appropriate access, correct the claim in the model.

A local review of each file will not necessarily catch these inconsistencies. Ask another compatible agent to examine the complete model and record discrepancies. Iterate on the affected parts until the narrative, evidence and audience fit together.

Do not start rendering slides while the governing argument is still being debated.

## Output: render and review the slides

Use [Design a slide]({{ site.baseurl }}{% link docs/playbooks/work.md %}#design-slides) where available, once for each agreed slide brief. Specify the approved Atlas visual style and current brand assets; do not mix unrelated design templates. For this example the output can be static HTML pages under `output/slides/`, with local images and stable element identifiers.

Review the slideshow in Workspace. Add comments to the specific text or graphic that needs refinement. You might ask for a simpler diagram, a clearer action title or more readable typography. These are changes to the rendering, not necessarily to the underlying argument.

![One reviewed presentation model can support different deliverables]({{ site.baseurl }}{% link assets/images/articles/presentation-views.svg %})

Use [Review a deck]({{ site.baseurl }}{% link docs/playbooks/work.md %}#review-deck) for an overall critique and [Prepare a talk track]({{ site.baseurl }}{% link docs/playbooks/work.md %}#prepare-talk-track) to develop speaker notes and likely questions. If an issue concerns an unsupported product claim, return to the model and research; do not merely make the slide sound more convincing.

When the HTML version is satisfactory, ask an agent equipped for the required output format to create the final deck, for example PowerPoint. The slide design playbook creates HTML, not a native PPTX file. Conversion is a separate task; inspect the converted result for visual fidelity, text, links and editability.

The same reviewed storyline can also support a short written briefing or meeting preparation notes. These are alternative views of the communication model, not independent opportunities to invent new claims.

## What this workflow buys you

You can change the audience, shorten the slot or incorporate a new product capability without throwing away the entire presentation. Update the relevant inputs and research, reconsider the affected model elements, then render the revised slides.

You can also divide the work among agents from different AI subscriptions: one for research, another for the storyline, another for independent review and another for slide generation. All use the same authorised Atlas workspace and persistent artifacts. Their client capabilities and subscriptions remain separate.

The value is not that Atlas automates every decision. It gives you a place to co-create, delegate, inspect and refine the work while preserving the understanding from which the presentation was produced.

For the underlying method, return to [A practical workflow for co-creation]({{ site.baseurl }}{% link docs/articles/structured-co-creation.md %}).
