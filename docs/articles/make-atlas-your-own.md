---
title: "Make Atlas your own: templates, playbooks and the library"
parent: Articles
nav_order: 9
description: "Reuse successful ways of working with AI, create project templates and customise shared playbooks in the Atlas library."
---

# Make Atlas your own: templates, playbooks and the library

After working with an AI agent on a few projects, you may notice that you keep explaining the same things. You ask it to organise the files in a particular way, follow a research method, check its findings and prepare a report in your preferred format.

You should not have to repeat these instructions every time. Atlas lets you retain a successful way of working as a project template, a playbook, or both. The library is where these reusable resources live.

This article builds on [Co-creating with AI]({{ site.baseurl }}{% link docs/articles/index.md %}), where we developed a local-history exhibition from source documents through research and modelling to a public presentation.

The library also makes working methods portable across compatible AI clients. A playbook lives in Atlas rather than only in one provider's custom instructions or conversation. You can use it with different connected agents without maintaining separate copies for every subscription. Models may interpret the same procedure differently, so test important playbooks with the clients you intend to use.

## Three things worth keeping separate

A project template supplies the starting material. A playbook describes how to work. The project itself holds the evolving result.

| | Project template | Playbook | Workspace project |
| --- | --- | --- | --- |
| Answers | What should we start with? | How should we do this work? | What are we working on now? |
| Contains | Folders, starter files, instructions and prompts | A procedure, decision points, reference material and expected outputs | Inputs, working material, decisions and deliverables |
| Reuse | Create another project from a copy | Refer to the shared procedure again | Continue the same project |
| Changes | Affect future projects created from it | Affect future reads of the shared procedure | Affect this project |

![The library supplies starting material and working methods to projects]({{ site.baseurl }}{% link assets/images/articles/library-reuse.svg %})

There is no requirement to use a template and a playbook together. You can start from an empty workspace and use a playbook for a particular task. You can also use a template simply to save time setting up a familiar folder structure.

## Start with an existing project template

Open Workspace and choose **New project from template**, or **New from template…** in the Open dialogue. Pick an available template, give the project a name and fill in the requested fields.

For example, the Knowledge base template asks for a title and subject. It creates the initial project with instructions and a handoff. Its setup playbook then guides you and the agent through agreeing on article types, folders, tags and the first articles.

A template is a starting point, not a permanent constraint on the project. After creation, you can change its copied files as you would any other workspace files.

Some templates provide reusable prompts in the project home. Others include starter plans, notes or references. What is available depends on the apps and library configured for your Atlas instance.

See the [project template catalogue]({{ site.baseurl }}{% link docs/playbooks/templates.md %}) for examples.

## Use a playbook to delegate a familiar task

A playbook is a written procedure for an agent. It can specify the information to collect, the sequence of work, when to consult you, which files to update and how to check the result.

For instance, the knowledge-base setup playbook does not simply tell an agent to create a collection of articles. It asks the agent to learn the subject, agree on conventions with you, create the KB, seed the first articles and verify the indexes.

You can ask a connected agent:

> Use the Set up a knowledge base playbook for this workspace. Ask me about the subject and intended audience before deciding on the structure. Do not create articles until we agree on the conventions.

The agent can discover applicable playbooks through its Workspace tools or fetch one by its reference, such as `atlas://playbook/setup-knowledge-base`.

Some longer playbooks are divided into stages, with reference material fetched as needed. Supported agents can record progress in the project and resume a staged run. This is useful when the work spans several conversations.

A playbook is not a deterministic automation script. An agent interprets its instructions, uses the tools it has permission to access and may need your judgement. Inspect intermediate results rather than assuming the procedure guarantees a correct outcome.

Browse the [playbook catalogue]({{ site.baseurl }}{% link docs/playbooks/index.md %}) to see what is already available.

## Turn a successful project into a reusable starting point

Suppose the local-history exhibition was successful and you expect to prepare another. You probably do not want to copy all its historical findings, draft text or open review comments. You want to retain the structure and the working conventions that made it successful.

Ask your agent:

> Review the completed exhibition project. Propose a reusable project template that preserves the folder structure, working instructions, starter brief and handoff, but removes the actual exhibition content and project-specific references. List any information that should be requested when a new project is created. Do not publish anything to the shared library yet.

Review the proposal in an ordinary workspace first. A simple template could contain:

```text
local-history-exhibition/
  template.yml
  AGENTS.md
  HANDOFF.md
  workspace.yml
  inputs/
    .gitkeep
  research/
    .gitkeep
  working/
    exhibition-brief.md
  output/
    .gitkeep
```

The template metadata describes what appears in the template picker and which values a new project needs. For example:

```yaml
name: Local history exhibition
description: Prepare a source-backed exhibition for a public audience.
variables:
  - name: subject
    label: Exhibition subject
  - name: audience
    label: Intended audience
    default: Local residents and school groups
```

In the starter brief, use placeholders:

```markdown
# Exhibition brief

Subject: {{subject}}
Audience: {{audience}}

## Goal
Agree on the scope and intended outcome.

## Questions to investigate
To be established during the initial review.
```

Atlas replaces declared placeholders in supported text files and file paths when creating the project. Built-in values include `project`, `date` and `user`. Binary files are copied unchanged. The template's own metadata file is not copied into the new project. Keep placeholders simple, and test values that contain punctuation if they appear in YAML files.

## Turn a successful method into a playbook

The exhibition process itself can also be reused. Unlike the template, it should describe how the agent and person proceed once the project exists.

Start with a short procedure in Markdown. For example, a single-file playbook might look like this:

```markdown
---
name: research-local-history
description: Use when researching a local history exhibition from supplied documents and further sources.
type: Playbook
tags: [history, research, exhibition]
reads: [AGENTS.md, HANDOFF.md]
---

# Research local history

## Inputs
The project brief and original source material in inputs/.

## Work
1. Inventory the sources and record extraction limitations.
2. Distinguish documented facts, interpretations and gaps.
3. Agree on research questions with the person.
4. Investigate the agreed questions and cite sources.
5. Build or update the agreed knowledge model.
6. Review claims and relationships before preparing public text.
7. Update HANDOFF.md with the result and outstanding work.

## Hands off
Original sources and unrelated project files.

## Done when
The evidence is traceable, unresolved questions are visible,
and the person has reviewed the model.
```

This is an illustrative custom playbook, not a shipped item. The description should say when the procedure is useful: agents discover playbooks by their names, descriptions and tags.

A good playbook gives the agent enough direction to do useful work without replacing your judgement. State the deliverables, review points, limits and completion criteria. Avoid embedding the results of a particular project into a general procedure.

If the procedure becomes long, put it in a folder with a `SKILL.md` entry point and separate reference files. A staged playbook can additionally declare its stages, dependencies and outputs. Start with a simple procedure; introduce stages when they help with substantial deliverables or resumable work.

You can link a template to a setup playbook by adding a field to its `template.yml`:

```yaml
setup: atlas://playbook/research-local-history
```

Only do so if that procedure really belongs at project creation. A later research or review task can instead be invoked independently.

## Where these resources live: the library workspace

The Atlas library is a configured workspace, or a set of layered workspaces. It holds shared resources that applications and agents can discover.

A typical library includes:

```text
project-templates/
  knowledge-base/
    template.yml
    AGENTS.md
    HANDOFF.md
  local-history-exhibition/
    template.yml
    ...
playbooks/
  setup-knowledge-base/
    SKILL.md
    stages/
    references/
  research-local-history.md
```

Your instance may also contain shared agent skills and other app-specific resources. The ordinary Workspace tools are used to edit library files. There is no need to develop an Atlas application to add a project template or a playbook.

Using a library resource does not require direct access to its underlying workspace. All users of the Workspace service can discover and read the configured templates and playbooks through the corresponding tools. Editing the library requires the relevant workspace grants.

Ask your administrator which library workspace is intended for custom content. A workspace with a folder named `playbooks` does not automatically become part of the shared library.

## Customise safely: use a separate library layer

Atlas supports layered library workspaces. An administrator can configure a shipped library followed by a custom library. A later layer takes precedence for a template or playbook with the same identifier.

This lets you keep local adaptations separate from Atlas-supplied content. Shipped library files can be refreshed during an operator's library update, so directly modifying a seeded file risks losing the change.

For most customisation, give your new resource a distinct identifier. Copy an existing template or playbook into the custom library, rename it and change its instructions or starting files. For a folder-based playbook, copy the entire folder so its relative references remain valid.

Only reuse a shipped identifier if you deliberately want to override it for everyone using that configured library. An override also means you must review future upstream improvements yourself.

| Need | Recommended approach |
| --- | --- |
| Change the instructions for one project | Edit that project's AGENTS.md |
| Change the handoff or working files | Edit the project itself |
| Reuse a starting structure for future projects | Create a custom project template |
| Reuse a method across projects | Create a custom playbook |
| Replace a shared default procedure | Review and publish an intentional library override |

Library content is shared, including the instructions agents receive from it. Give edit access only to trusted maintainers and never put secrets, private project data or client-specific information into shared templates and playbooks.

## Test before sharing

Create a disposable project from the new template. Check that its variables are substituted, the files are in the right places and the starter instructions make sense.

Then ask an agent with no previous project context to use the custom playbook. Inspect the resulting artifacts and check whether it follows the intended review points. Ask it to report ambiguities rather than filling gaps silently.

Improve the resource based on the test. Publish it to the configured custom library only when it is ready for other users.

Remember that template updates do not rewrite existing projects. Playbooks are read from the shared library rather than copied into every project, so a change can affect future work in existing projects. Finish or review active staged runs before changing their procedure; supported staged clients detect changed playbook revisions.

## Build your own way of working

Templates and playbooks are not meant to impose a single process on every project. They let you preserve the parts of your collaboration that work well and change the parts that do not.

Begin with a project. Co-create something useful, notice which instructions or structures recur, and turn those into reusable resources. Keep them in the library so the next project can start with the experience of the previous one.

Read [Continuing work across conversations and agents]({{ site.baseurl }}{% link docs/articles/co-creating/continuing-work.md %}) for project-specific instructions and handoffs, and [From raw material to finished work]({{ site.baseurl }}{% link docs/articles/co-creating/from-inputs-to-output.md %}) for the workflow used in this example.
