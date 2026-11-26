---
title: "A practical workflow for co-creation"
parent: Articles
nav_order: 1
description: "Separate input interpretation, research, modelling and rendering to build and refine substantial work with AI."
---

# A practical workflow for co-creation

A substantial piece of work rarely begins with a complete specification. You may know what you want to achieve, but not yet what the evidence supports, how the result should be structured or how to communicate it.

Asking an AI agent to research a topic, invent a solution and write the final report in one instruction mixes several different kinds of work. A plausible-looking result can conceal a misunderstood requirement, an unsupported claim or incompatible assumptions.

Atlas — The Light Factory supports a different approach: build understanding first, develop an explicit model of the intended result, and render that model for its audience. Inspect and refine the work at each stage. Delegate bounded tasks autonomously where useful; use your expertise where the direction or quality of the work depends on it.

![Separate the work into stages, with review and refinement throughout]({{ site.baseurl }}{% link assets/images/articles/structured-workflow.svg %})

## The basic collaboration loop

At any stage, give the agent the relevant context and ask it to draft or change a specific artifact in Workspace. Inspect the result. Edit it yourself if that is quicker, leave comments where a local change needs interpretation, or use a refinement prompt for broader changes. Ask the agent to address the comments, reply and resolve only those it has dealt with.

This is the same loop whether you are reviewing an input summary, a research article, a model or a finished slide. See [Working together in a shared workspace]({{ site.baseurl }}{% link docs/articles/co-creating/shared-workspace.md %}) for the detailed interaction.

## 1. Create a workspace

Start a new workspace for a substantial new undertaking. A workspace is inexpensive to establish compared with reconstructing context from scattered conversations and files. Use an appropriate [project template]({{ site.baseurl }}{% link docs/playbooks/templates.md %}) if it gives you a useful starting structure; otherwise begin with an empty project.

For a substantial piece of work, the following structure is a useful convention:

```text
my-project/
  AGENTS.md
  HANDOFF.md
  inputs/
    instructions.md
  report/
  research/
  model/
  output/
```

These folder names are a suggested working method, not reserved Atlas directories. Existing templates and playbooks may use different paths. Keep their conventions or adapt them deliberately, and tell the agent where each stage's artifacts belong.

## 2. Describe the intended result in AGENTS.md

Start by recording what you want to build: its purpose, audience, scope, constraints and the criteria by which you will judge success. Include the working rules that should remain applicable across conversations.

An initial instruction file might say:

```markdown
# Project goal

Develop a technical presentation about Atlas for an audience
of solution architects. Explain the product approach and
how people and agents work with persistent artifacts.

# Working method

- Preserve original inputs.
- Summarise supplied material in report/.
- Research in research/; cite evidence and mark gaps.
- Construct and validate the intended result in model/.
- Render reviewed models into output/.
- Ask before changing the intended scope.
- Update HANDOFF.md at meaningful checkpoints.
```

You do not need to know all the requirements yet. Revise the goal when exploration changes your understanding, and make that change explicit.

Keep detailed task-specific directions in `inputs/instructions.md`, rather than continually expanding the standing project instructions. [Continuing work across conversations and agents]({{ site.baseurl }}{% link docs/articles/co-creating/continuing-work.md %}) explains how to maintain instructions, handoffs and project context.

## 3. Collect the inputs

Put the material you already have into `inputs/`: your own brief, examples and templates, emails, meeting notes, chat logs, reference documents and links. Include constraints or preferences that might otherwise exist only in your head.

A source template may tell you what shape the eventual result should take; an email may state a requirement; a chat log may contain an earlier decision. They are not necessarily equally authoritative. Record their origin and distinguish instructions you endorse from statements in the supplied material.

Use Workspace intake for supported non-native files. It keeps originals alongside accessible extracted material and available previews. See [Working with documents beyond Markdown]({{ site.baseurl }}{% link docs/articles/co-creating/working-with-documents.md %}) for the details.

Collect only material you are authorised to share with the agents and people working in the project.

## 4. Interpret the inputs into a report

Before asking for additional research or a solution, ask the agent to make the supplied material navigable.

> Read AGENTS.md and the contents of inputs/. Create a structured account of the supplied material in report/. Use separate OKF-style Markdown articles with titles, consistent tags, source references and links between related topics. Identify stated requirements, facts, preferences, contradictions and open questions. Do not perform external research, resolve conflicting requirements on my behalf or propose the final solution.

The report is an interpretation of the inputs, not a replacement for them. It reduces the need to reread lengthy attachments and gives you a place to correct the agent's understanding.

An OKF-style article can be as simple as a Markdown file with an Atlas-compatible metadata header:

```markdown
---
Title: Presentation audience and constraints
Tags: audience, requirements
---
The supplied brief identifies solution architects as the audience.

## Open questions
The available presentation slot is not yet specified.

## Sources
- [Original instructions](../inputs/instructions.md)
```

Agree on a small vocabulary and a clear file layout. Add an overview linking the articles. Agents can navigate these ordinary files directly; KB Explorer can also inspect a workspace folder as a graph view when the articles declare supported relationships. A formal KB is optional and has its own indexing and editing conventions.

**Review before proceeding:** Does the report represent what you supplied? Have requirements been confused with assumptions? Are important source details or contradictions missing?

## 5. Research without designing the answer

Use the reviewed report to define what must be investigated. Give the agent the sources it should consult: permitted web research, an existing KB, specific Search datasets, documentation or other accessible collections.

> Investigate the questions in report/. Save findings as linked OKF-style articles in research/. For each substantive claim, record the original source, relevant passage or section and any limitations. Distinguish source claims from your summaries; preserve conflicting findings. At this stage do not invent facts, fill gaps with speculation, construct a proposed solution or draft the final deliverable. Report questions that remain unanswered.

A research article should link to the evidence, not simply mention that research was performed. For a web source, record its URL and relevant date where available. For an internal file, reference the file and its page or section. For Search results, follow the source reference when possible instead of treating a ranking score as proof. For a KB article, retain its original citations where relevant.

Research can include comparing accounts and evaluating the reliability of evidence. The boundary is that it should not silently turn those findings into a new design or invented domain content.

Choose the research tool according to the source: KB for structured knowledge you can navigate, Search for semantic retrieval across larger collections, and external research tools where your client supports them. See [KB versus Search — which one to use?]({{ site.baseurl }}{% link docs/articles/kb-versus-search.md %}).

**Review before proceeding:** Is there enough evidence to start modelling? Can important findings be traced to their sources? Are disagreements and gaps visible? You may choose to proceed with a known limitation, but record it explicitly.

## 6. Model the intended result

Now the agent has a different task. It may analyse, synthesise, propose alternatives and construct a solution. Ask it to build the working representation in `model/`, using the reviewed inputs and research as constraints.

A model need not be a diagram or a simulation. It can be a collection of Markdown documents describing an architecture, a set of narrative messages for a presentation, a policy proposal or an account of a fictional world. Where appropriate, supplement it with UML diagrams, causal models, plans, maps or other Atlas artifacts. Use the corresponding app to edit its managed document rather than treating its internal files as ordinary prose.

### Model sub-domains independently

For a complex undertaking, identify its meaningful parts. For example, a technical presentation might have separate models for audience needs, the key argument, supporting technical material and the slide storyline.

For each part, ask the agent to:

1. Read the relevant inputs and research.
2. Construct the sub-model with explicit assumptions and references.
3. Check its correctness and completeness against the evidence.
4. Present the result for review and refine it as needed.

Use folders to organise the sub-domains. [Bundles]({{ site.baseurl }}{% link docs/articles/co-creating/bundles.md %}) can make each folder easier to browse in the UI. Bundling changes presentation, not the agent's reasoning or access to the files; provide an explicit overview and instructions if the agent needs them.

### Consolidate the whole model

A collection of individually correct sub-models is not necessarily a coherent whole. They may have adopted different definitions, overlapping responsibilities, incompatible assumptions or conflicting timelines.

Once the sub-domains are locally satisfactory, review them together:

> Review the full model against the inputs and research. Identify contradictions between sub-domains, inconsistent terminology, duplicated responsibilities and missing interfaces or relationships. Record the issues and propose targeted changes. Do not hide differences by rewriting the overview alone.

Resolve the differences by incrementally revising the affected sub-models, checking their local correctness again and repeating the cross-model review. Keep unaffected material intact.

![Local modelling and whole-model consolidation]({{ site.baseurl }}{% link assets/images/articles/model-consolidation.svg %})

An independent agent can be particularly useful for this review. Because Atlas is model-agnostic, you can use compatible agents from different subscriptions on the same saved artifacts, without transferring the project into another provider's conversation.

**Exit modelling when:** the model satisfies the reviewed requirements, is supported by the research or explicitly justified assumptions, and is consistent across sub-domains. The standard is fitness for the intended purpose, not the elimination of every conceivable uncertainty. If the model reveals an evidence gap, return to research and record the new finding there.

## 7. Render the model into output artifacts

Once the model is satisfactory, ask the agent to produce the deliverables in `output/`. A single model can support several views: a detailed report, a short article, a presentation, a meeting brief or supporting notes, each tailored to its audience.

> Read the reviewed model and render the agreed deliverables in output/. Apply the intended audience, format and visual style. Preserve traceability to the model and evidence. Do not introduce new substantive claims or silently change modelling decisions while preparing the output.

For presentations, use the available presentation playbooks to render static HTML slides first. Inspect them in Workspace's slideshow view, comment on specific elements and refine the slides. Then ask an agent with the necessary conversion or document-generation capability to produce the preferred final format. Check that conversion preserves the content and appearance; HTML preview is not itself a guarantee of editable PowerPoint output.

Review the output for both communication quality and fidelity to the model. A wording or layout problem can usually be corrected in the output. A substantive change belongs in the model first, and then in all affected outputs.

See [Going beyond documents]({{ site.baseurl }}{% link docs/articles/co-creating/beyond-documents.md %}) for the available representations and [Creating a technical presentation with AI]({{ site.baseurl }}{% link docs/articles/technical-presentation.md %}) for a worked example.

## Why separate the stages?

| Stage | The agent's focus | What you inspect |
| --- | --- | --- |
| Inputs | Preserve supplied material | Coverage and authority |
| Report | Interpret what was supplied | Correct understanding of requirements |
| Research | Find and evaluate evidence | Provenance, reliability and gaps |
| Model | Analyse and construct | Correctness and cross-domain consistency |
| Output | Communicate the reviewed model | Fidelity, audience fit and presentation |

The boundaries make instructions more precise: an agent asked to investigate evidence is not simultaneously asked to invent a solution. A model can be reviewed before polished output makes its assumptions harder to see. Errors can be corrected at their source, and useful work preserved when requirements change.

This is not a mandatory waterfall. Return to earlier stages whenever a finding or review changes the problem. Smaller tasks may need only a brief, a draft and a review. For substantial bespoke work, keeping the stages and their artifacts distinct makes collaboration cumulative, inspectable and transferable between agents.

When the method becomes familiar, [turn it into a template or playbook]({{ site.baseurl }}{% link docs/articles/make-atlas-your-own.md %}) for the next project.
