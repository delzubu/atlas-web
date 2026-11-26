---
title: "UX guide"
parent: Design
grand_parent: Developers
nav_order: 3
---

# UX guide

This guide is the human-readable website edition of the [source specification](https://github.com/delzubu/atlas-core/blob/4a656981ba21c6139a3d3750e0cb76f8d706284a/docs/design/ux.md). It preserves the design rules and their identifiers; implementation status and measurements are those recorded in the source, not a fresh audit.

How an app's experience is designed so that it behaves like Atlas. [ui_design.md]({{ site.baseurl }}{% link docs/developers/design/ui_design.md %}) says how the product looks and where
things sit; [mobile.md]({{ site.baseurl }}{% link docs/developers/design/mobile.md %}) says how it works on small screens; this file says **how a person works with it**: the flow, how
screens are cut, how apps connect, how the agent takes part, and what must be fast. Read it before you design or change an app's
screens or flows, and run the [checklist](#checklist) before you call the work done.

**Status: first version, 6 Nov 2026.** Rules are numbered `UX-n` so reviews, commits and agents can cite them. Response-time numbers
(see Speed) are **design targets, not measurements**. The [gaps](#gaps-today) are what a read of the code found; they are not fixed by
this file.

## How to apply this guide

Begin by describing the person's intended outcome and the sequence of decisions or actions needed to reach it. Next, decide which content must remain visible while they work. Only then choose the shared components and design the individual screens. The numbered rules below are retained so a design review can refer to a specific requirement, rather than merely expressing a preference.

The checklist is a design-review instrument, not a claim that every existing app already meets all 38 rules.

## Relation to the other guides

| Guide | Owns |
|---|---|
| [product-brandbook.md]({{ site.baseurl }}{% link docs/developers/design/product-brandbook.md %}) | Identity: positioning, logo, brand palette |
| [ui_design.md]({{ site.baseurl }}{% link docs/developers/design/ui_design.md %}) | Look and structure: tokens, the four regions, type, icons, where commands live, loading, voice, keys |
| [mobile.md]({{ site.baseurl }}{% link docs/developers/design/mobile.md %}) | Small screens and touch: targets, Action bar, Sheet, FocusTask |
| [components/]({{ site.baseurl }}{% link docs/developers/design/components/index.md %}) | Anatomy of each shared component |
| **ux.md** (this file) | Flow and behaviour: the journey, focus, steps, connected workflows, references, agents, speed, devices |

- **UX-1.** Do not restate another guide; state the UX intent in a line and link. A rule lives in one file.
- **UX-2.** Conflicts: this file wins for flow and behaviour, `ui_design.md` for look and structure, `mobile.md` below 1200 px and on
  touch, the brand book for the logo and brand hexes.
- **UX-3.** If the user flow does not settle a design question, **ask the owner. Never assume a flow.** This holds for people and for
  agents designing a screen.

## What an Atlas app is

An app is an instrument for **viewing and editing content**: a plan, a model, a map, a note, a register. Everything else (chrome,
menus, dialogs) exists to serve that and to get out of the way. It follows the brand book's beliefs as UX consequences:

- Iteration is first-class, so every edit can be undone and every change compared (UX-31).
- Work outlives the conversation, so the result is a file or record in a workspace or store, never only a chat answer.
- People inspect and correct, so everything an agent does is visible, attributed and reversible (UX-29).

## The suite journey

Work usually runs like this, and a new app must say where it joins:

1. **Pick a workspace** (a project): Launcher → Continue, or All projects.
2. **Check what is mine**: the Today strip and [My work]({{ site.baseurl }}{% link docs/developers/design/components/MyWork.md %}).
3. **Work on items in place**: a plan, a register, a model, a knowledge base.
4. **Update the documents** of the workspace.

Throughout, the person **co-creates with an agent** (UX-26 to UX-30): give input, the agent drafts, the person reviews, the agent updates.

- **UX-4.** State in the app's `AGENTS.md` where the app is entered from: the project home, My work, a link, the launcher. If it is
  none of these, it is probably not an Atlas app.
- **UX-5.** Every app carries the project: `?project=&ref=` in the URL, the project as the root of the breadcrumb, a way back to the
  project home. Opening the app from a link lands on the item, never on a welcome page.
- **UX-6.** A document app stores its documents in a workspace folder with an `about.json`
  ([document-descriptor](https://github.com/delzubu/atlas-core/blob/4a656981ba21c6139a3d3750e0cb76f8d706284a/docs/specs/document-descriptor.md)) and declares them in the catalog. A first run starts from a template, an
  example or the agent, never from a blank page.

## Content in focus

- **UX-7.** Content gets the screen. The editing surface is the default or one key away (`E`), and editing happens **in place**, in the
  view. There is no separate edit page.
- **UX-8.** **Details supplement the content; they never replace it.** Edit details in the side panel (the Inspector), so the content
  stays visible while you edit. Where a panel cannot do the job, use a **movable, dockable popup** that leaves the content partly
  visible. Navigating to a separate "details" view is not acceptable.
- **UX-9.** Combine by frequency: the most common fields in the panel, the full set in a popup opened from it.
- **UX-10.** Decide what is easy to edit from the user flow. The designer lists the fields, says which are one action away and which
  may sit on another surface, and writes the reason down in the app's `AGENTS.md`. If the flow does not settle it, ask (UX-3).
- **UX-11.** Chrome never blocks reading: nothing floats over the text except what the person summoned and can move or dismiss (the
  chrome budget in [ui_design.md]({{ site.baseurl }}{% link docs/developers/design/ui_design.md %}), Layout).

## One workflow step per screen

- **UX-12.** A screen serves **one step** of one workflow: read the list, edit the plan, review the changes, answer the question. It is
  not a cockpit that shows every capability at once.
- **UX-13.** A *cockpit* is several steps shown at once, or a dashboard of unrelated tasks. It is not "several regions": the four-region
  shell stays. The Inspector holds context for the **current** step only.
- **UX-14.** Moving between steps happens in place (an Inspector tab, a Sheet, the agent window), not as a page load. Each step keeps
  the person's place: URL, selection, scroll, unsaved input.
- **UX-15.** Each step the person may return to or share has its own URL or history entry, so back and forward go where they expect.
- **UX-16.** Write the app's workflow as a list of steps in its `AGENTS.md` before building screens. A step with more than one primary
  action, or more than three primary actions in the header, is probably two steps.

## Commands

[ui_design.md]({{ site.baseurl }}{% link docs/developers/design/ui_design.md %}) (Commands: where they live) gives each command its home. The UX rules on top:

- **UX-17.** Common commands are **always visible**, without scrolling and without ⌘K: create, edit, delete, **open another item**,
  copy link, ask the agent. The palette holds every command as well, but never the only way to a common one.
- **UX-18.** **Open another item** is reachable from every screen of the app, in the navigator tools or the DocHeader/⋯ menu, without
  scrolling the menu. It offers recents first and a search over the project.
- **UX-19.** A disabled command says why in its tooltip ("No plan open"). A command the person may not use (UX-36) is not shown.

## Connected workflows

Atlas is a suite: the value is in the moves between apps. The person never has to leave the item in hand to find another.

- **UX-20.** When another app is deployed **and** assigned to the person (`useCapabilities()`, from `/api/me/config`), the app offers
  the cross-app action where it is needed, in context. Examples: **search the knowledge base from a document** (Look up); **link** a
  person, a plan item, a model element or a KB article from a document; **Open in &lt;App&gt;**; **New &lt;document&gt; here**.
  Hand-offs carry the context (project, item, selection).
- **UX-21.** Never offer an action that would fail. Every reference degrades along the ladder in
  [connected-suite §3.5](https://github.com/delzubu/atlas-core/blob/4a656981ba21c6139a3d3750e0cb76f8d706284a/docs/plans/2026-10-17-connected-suite-ux.md): open in the app → stored preview → label and icon → label only.
  An app declares its relations in the catalog (`uses`, `documents`), never by importing another app (see the dependency rules in
  the root `AGENTS.md`).

### References

- **UX-22.** **Inside Atlas, every reference between items is an `atlas://` URI** ([atlas-uri](https://github.com/delzubu/atlas-core/blob/4a656981ba21c6139a3d3750e0cb76f8d706284a/docs/specs/atlas-uri.md)). The UI stores,
  shows, copies and resolves URIs, not web addresses.
- **UX-23.** A reference to a typed document, or to an object in one, **names its tool** with the `open` hint and the object fragment:
  `atlas://workspace/acme/main/plans/wbs?open=planner#t12`. The store stays the authority (the spec's rule, unchanged), so the link
  opens in Planner when the person has it and falls back to the stored preview when not.
- **UX-24.** Relative references are allowed only **inside the same workspace and app**.
- **UX-25.** Show a reference with `RefView` and pick one with the ref picker. **Copy link** copies the `atlas://` URI. The https form
  (`resolveRef`) is for places that cannot resolve Atlas links: exports, e-mail, external systems.

## Co-creation with agents

For a practical illustration of these rules in everyday work, see [A practical workflow for co-creation]({{ site.baseurl }}{% link docs/articles/structured-co-creation.md %}) and [Working together in a shared workspace]({{ site.baseurl }}{% link docs/articles/co-creating/shared-workspace.md %}). These articles describe how people can use the same artifacts with compatible agents across subscriptions; the rules below describe the experience an Atlas app should provide.

The usual loop: **the person gives input → the agent drafts → the person reviews (comments, or an instruction) → the agent updates**,
and again. The agent is the suite's companion ([Assistant]({{ site.baseurl }}{% link docs/developers/design/components/Assistant.md %})); an external agent over MCP does the same work.

- **UX-26.** **Parity:** every content operation a person can do is an MCP tool, and the reverse, so the companion and any other agent
  can do what the person can. The tools declare their hints and the plugin skills name them (root `AGENTS.md`).
- **UX-27.** The app tells the companion where the person is (app, project, open document, selection). Nobody is asked for it.
- **UX-28.** **Review happens in the artifact:** comments anchored to the content ([review-comments](https://github.com/delzubu/atlas-core/blob/4a656981ba21c6139a3d3750e0cb76f8d706284a/docs/specs/review-comments.md)) that
  the person and the agent can list, answer and resolve, plus instructions in the agent window. A draft is not final until a person
  accepts it: agent proposals are marked `suggested` until confirmed.
- **UX-29.** Agent writes are attributed and visible: "updated by" with the actor, a change banner, a link to what changed and an Undo.
  Nothing reloads silently; the app finds changes when the person returns (`useOnReturn`), without polling.
- **UX-30.** **MCP results carry an `atlas://` `uri`**, with the `open` hint (UX-23), and that is what every Atlas surface uses, the
  companion's answers included (rendered as `RefView`). **`web_url` exists for conversations outside Atlas** (opening an item from
  ChatGPT or Claude). The Atlas UI never displays, stores or links a `web_url`.

## Iteration and recovery

- **UX-31.** Every edit, by a person or an agent, can be undone. History shows change sets. Conflicts show the field-level dialog
  ([Forms]({{ site.baseurl }}{% link docs/developers/design/components/Forms.md %}); [workspace-concurrency](https://github.com/delzubu/atlas-core/blob/4a656981ba21c6139a3d3750e0cb76f8d706284a/docs/specs/workspace-concurrency.md)). Unsaved input is never dropped; leaving
  with unsaved work asks.
- **UX-32.** A destructive action either confirms or can be undone, never silently destroys.

## Speed

User workflow and response time are first-class. These are **targets** until someone measures them; measure before you claim them.

| Target | Value |
|---|---|
| Visible response to input (press, click, key) | ≤ 100 ms |
| Switch to another item: cached content or a skeleton | immediately |
| Switch to another item: usable | ≤ 1 s on a laptop on the hobby profile |
| Typing in an editor | never blocked by a save |

- **UX-33.** No full-page reload inside an app; moving between items and steps is in place (UX-14).
- **UX-34.** Show what you have at once (cache, marked "as of 10:12") and refresh in the background. A failure affects only its region
  ([ui_design.md]({{ site.baseurl }}{% link docs/developers/design/ui_design.md %}), Loading and failure). Edits apply locally first; the save is quiet and visible.
- **UX-35.** No polling timers. Check on return (UX-29), or when the person acts.

## Access

- **UX-36.** The screen follows the person's role (`reader`, `reviewer`, `writer`, `admin`; [authorization](https://github.com/delzubu/atlas-core/blob/4a656981ba21c6139a3d3750e0cb76f8d706284a/docs/specs/authorization.md)).
  A reader sees a read-only view with no dead buttons. A reviewer can comment but not edit. A command the person may not use is hidden,
  not disabled (UX-19).

## Help and empty states

- **UX-37.** One-line empty states that name the next action ([ui_design.md]({{ site.baseurl }}{% link docs/developers/design/ui_design.md %}), Voice). Ship `help/*.md` for every screen and
  change it in the same change as the screen ([Help]({{ site.baseurl }}{% link docs/developers/design/components/Help.md %})).

## Devices follow the job

Work almost always **starts on a computer**: creating and co-creating assume a laptop screen, a keyboard and a pointer. Other devices
get a narrower job, and the design offers exactly that.

| Device | Job | Consequence |
|---|---|---|
| Laptop | Create, edit, co-create with the agent | The full experience. The default target |
| Tablet | Read, view, minor updates | Touch limits typing: no long text; edits are toggles, pickers, short fields and comments |
| Phone | Review and confirm; test execution (Study, Languages) | Select an option or type one word: approve or reject, answer a comment, pick, short answers |

- **UX-38.** An app declares its level (`laptop`, `touch`, `phone-first`) in its `AGENTS.md` and says which screens serve which job.
  [mobile.md]({{ site.baseurl }}{% link docs/developers/design/mobile.md %}) says how; this table says what each device is for. Do not build long-form creation for a phone, and do not
  drop review and confirmation from it.

## Checklist

Run it before an app's UX is called done, and put the result in the change summary. "n/a" needs a reason. Each item cites its rule.

**Journey and focus**
- [ ] The app's `AGENTS.md` says where it is entered from, and its workflow as steps (UX-4, UX-16)
- [ ] The URL carries `project` and `ref`; a link lands on the item; the project is the breadcrumb root (UX-5, UX-15)
- [ ] Documents are workspace folders with an `about.json`, declared in the catalog (UX-6)
- [ ] Content is the default view; editing is in place (UX-7)
- [ ] Details edit in the Inspector, or a movable popup when it cannot; nothing navigates away to a details view (UX-8)
- [ ] The most common fields are in the panel, the rest in a popup; the reasons are in `AGENTS.md` (UX-9, UX-10)
- [ ] Nothing floats over the text unasked (UX-11)

**Steps and commands**
- [ ] One workflow step per screen; no cockpit (UX-12, UX-13)
- [ ] Moves between steps happen in place and keep the place (UX-14)
- [ ] Common commands are visible without scrolling; **Open another item** is in the menu on every screen (UX-17, UX-18)
- [ ] Disabled commands say why; forbidden ones are hidden (UX-19, UX-36)

**Connected**
- [ ] Cross-app actions appear when the other app is available to the person, and only then (UX-20, UX-21)
- [ ] All references are `atlas://`, document references carry `open`, relative ones stay inside one workspace and app (UX-22 to UX-24)
- [ ] `RefView` and the ref picker are used; Copy link copies the `atlas://` URI (UX-25)

**Agents**
- [ ] Every content operation exists as an MCP tool and in the UI (UX-26)
- [ ] The companion gets the person's context (UX-27)
- [ ] Review comments are anchored; agent proposals are `suggested` until confirmed (UX-28)
- [ ] Agent writes show who, what, and Undo; no silent reload, no polling (UX-29, UX-35)
- [ ] MCP results carry `uri`; `web_url` is never shown in the UI (UX-30)

**Speed and safety**
- [ ] Response targets measured, or stated as not measured (Speed)
- [ ] No page reload inside the app; cached content first; failures contained to a region (UX-33, UX-34)
- [ ] Every edit is undoable; destructive actions confirm or undo; unsaved input is protected (UX-31, UX-32)
- [ ] A secret that has a value is shown masked (`••••` + last 4), never as an empty box, wherever the value comes from (`components/Forms.md`, Secret fields)

**Devices, help, voice**
- [ ] The target level is declared and screens are assigned to jobs (UX-38)
- [ ] Empty states name the next action; the help pages changed with the screens (UX-37)

## Gaps today

What a read of the code found on 6 Nov 2026. Copy link (UX-23, UX-25) was a gap and is fixed: `useAtlasLinks().copy` copies the `atlas://` URI with the `open` hint. It was a targeted read, not a full audit; **unverified** means not checked. Fix a gap in the
first change that needs it and update this list.

| Gap | Rule | Where | Confidence |
|---|---|---|---|
| MCP results carry both `uri` and `web_url`; the UI side of `web_url` (`describe.ts`, and edition apps' own types) is not checked for display | UX-30 | `apps/*/api/**/mcp_server.py`, `packages/app-suite/src/refs/describe.ts` | Medium: the field exists; display unverified |
| "Open another item" in the menu on every screen; details editing that navigates away; MCP/UI parity; role-hidden commands | UX-18, UX-8, UX-26, UX-36 | all apps | **Unverified**: needs a per-app review |
| No response-time measurements exist | Speed | – | High (none found in `docs/`) |
