---
title: Launcher
parent: Component reference
grand_parent: Design
nav_order: 14
nav_exclude: true
---

# Launcher (the start page)

[Component reference]({{ site.baseurl }}{% link docs/developers/design/components/index.md %}) · [Source specification](https://github.com/delzubu/atlas-core/blob/4a656981ba21c6139a3d3750e0cb76f8d706284a/docs/design/components/Launcher.md)

The launcher at `/apps/atlas/` is the first screen of Atlas and the start tab of the installed app. It answers three questions, in this order, and the cheapest answer renders first:

1. **What needs me today?** The Today strip.
2. **Where was I?** Continue.
3. **Where do I go?** Tools.

It is the one product screen with brand presence ([ui_design.md]({{ site.baseurl }}{% link docs/developers/design/ui_design.md %}), "Where identity colour may appear"): the horizontal logo, `brand` eyebrows and one `brand-highlight` underline. Everything interactive uses `accent`, as everywhere else.

Preview: [previews/Launcher.html]({{ site.baseurl }}{% link docs/developers/design/previews/Launcher.html %}).

## Views

One bundle, three hash views. Back and forward move between them; in the installed app they stay in the start tab.

| Hash | View |
|---|---|
| (none) | Start page (this spec) |
| `#work` | [My work]({{ site.baseurl }}{% link docs/developers/design/components/MyWork.md %}), loaded as a separate chunk only when opened |
| `#projects` | All projects: the personal, pinned and recent projects as cards, each with its star (today's "Projects & Review" list, moved) |

## Anatomy (signed in, ≥1200px)

```
┌──────────────────────────────────────────────────────────────────────────────┐
│ [logo 40px]                                                         (PM) ▾   │  header 56px, surface, border below
├──────────────────────────────────────────────────────────────────────────────┤
│ Thursday 30 October                                                          │  title
│ ⌣ underline 56×8, brand-highlight                                            │
│                                                                              │
│ TODAY                                       Open My work →  │ TOOLS          │  eyebrows: brand
│ [⚠ 3 Overdue] [● 5 Due today] [◐ 4 Underway] [✎ 2 To answer]│ ▦ Planner    › │
│ ○ Call the bank          home › My TODOs         ⚠ 20 Oct   │ ⟲ Model      › │
│ ◐ API review             q4-launch › Plan  40%   Today      │ ◫ Diagram    › │
│ ↻ Weekly report          home › My TODOs         Today      │ …              │
│ 7 more in My work →                                         │                │
│                                                             │                │
│ CONTINUE                                  All projects 18 → │                │
│ home           ★  You · 2 h ago            ● Changed  2 ✎   │                │
│ garden-plans  ☆  Claude for you · yesterday                │                │
└──────────────────────────────────────────────────────────────────────────────┘
```

- **Page:** `main` is `min(1120px, 100% − 32px)` wide and centred, padded `space-5` at the top. The body is a grid `minmax(0, 1fr) 320px` with a `space-5` gap: the left column holds Today and Continue, the right holds Tools.
- **Below 1200px:** one column, in the order Today, Continue, Tools; Tools becomes a two-column grid of the same rows. Below 560px, the tiles become a 2×2 grid.
- **Below 768px (a phone):** the tools whose catalog entry lists `phone` in `web.targets` (Languages today) come first in Tools, every other tool keeps its order (`phoneFirst` in `launcher/model.ts`). The installed app also offers the shortcut "Practise a language" (`/apps/lang/?start=practice`), which opens the lessons of the first pair.

### Header

- Height 56px, `surface`, a `border` hairline below. It scrolls with the page.
- **Left:** the horizontal logo (`assets/atlas-logo-aubergine-coral.svg`), 40px high (the minimum for the horizontal logo is 120px wide; 40px keeps "The Light Factory" legible), aligned with the content column. It links to the start page.
- **Right:** the account button, a 32px circle in `surface-2` with the person's initials (`ui-strong`). It opens a `SuiteMenu` with:
  - the email (`meta`, not a command)
  - **Install Atlas**, only when the browser offers it
  - **Sign out**
- No tagline, no "about" text, no install hint for a signed-in person.

### Date line

The local date as "Thursday 30 October" in `title` (20/26, `fg`). Below it, the curved underline: the logo's underline path, 56×8px, `brand-highlight`. This is the screen's only coral.

### Today strip

- **Heading row:** the eyebrow "Today" (`meta`, uppercase, 0.12em tracking, `brand`). At the right, the link **Open My work →** (`accent`).
- **Tiles:** four in a row, equal width, `space-2` gap. Each is 72px high, `surface`, a `border` hairline, `radius-lg`, padding `space-3`. It holds the numeral (24px, 600, tabular figures) above the label (`meta`, `muted`). The glyph sits before the numeral.

| Tile | Glyph | Numeral colour | Counts | Opens |
|---|---|---|---|---|
| Overdue | ⚠ | `danger` | My items with `due` before today | `#work?show=overdue` |
| Due today | ● `warning-border` | `fg` | My items with `due` today | `#work?show=today` |
| Underway | ◐ | `fg` | My items with status `in_progress` | `#work?show=underway` |
| To answer | ✎ | `fg` | Comment threads waiting on me, over the personal and pinned projects | Workspace, the first project with any |

  - "My items" is the **mine** class of the worklist spec, over the personal and pinned projects.
  - A zero shows "0" in `muted`. The whole tile is the link; hover shows `hover`; focus shows the `accent` ring.
- **Next up:** at most 3 of my items (overdue, then due today, then underway; by due date, then title). Each is one 28px row:
  - left: status glyph, then title (`ui`)
  - middle: `project › plan` (`meta`, `muted`)
  - right: the progress percentage for a task; the due chip
  - The due chip reads "⚠ 20 Oct" in `danger` when overdue, "Today" after a `warning-border` dot when due today, and "20 Nov" in `muted` otherwise.
  - The row opens `#work?item=<uri>`.
- Then "N more in My work →" when there are more.

### Continue

- **Heading row:** the eyebrow "Continue" (as Today). At the right, **All projects N →**, which opens `#projects`.
- **Rows:** the 6 most recently opened projects, with no pinned projects mixed in. Each row is 40px, with a `border` hairline between rows:
  - the project name (`ui-strong`; `@ ref` in `muted` when the ref is not `main`)
  - the pin star
  - who changed it last and when (`meta`, `muted`; `actorLabel` · `ago`)
  - at the right: "● Changed" (an `accent` dot plus the word) when someone else changed it since I looked, and "2 ✎" for comments waiting on me
- A project that cannot be read any more shows "Cannot be opened any more" in `muted`.

### Tools

- **Heading:** the eyebrow "Tools".
- **Rows:** one row per tool the account has. Each row has:
  - a 28px `surface-2` tile (`radius-md`) holding the tool glyph, 20px, in `fg`
  - the label (`ui`)
  - a chevron `›` in `muted`
  - A tool that is not set up keeps its status badge ("Not configured").
- **Two densities**, chosen by the page's state (see First run):
  - **Compact** (Normal): 36px rows. The description is the row's `title` tooltip.
  - **Guided** (First run, Set up partly): 52px rows. The description is shown under the label (`meta`, `muted`), and the New action is always visible.
- The row is a link that opens the tool; in the installed app it opens a tab. Hover shows `hover`.

#### Starting a tool: the New action

A tool opened with no document shows its own picker, and that picker needs a project. So a row also offers to **create** the tool's first document, in one step.

- **Which tools have it:** each tool with `documents` in the catalog (`packages/catalog/src/atlas_catalog/apps.json`). The label is "New" plus the document's `noun`: **New plan**, **New model**, **New diagram**. No tool is named in the launcher's code.
- **What it opens:** the tool's create form, through the deep link every document tool already reads: `?project=<project>&ref=main&new=<folder>`, where the folder comes from the catalog (`.plans`, `.models`, `.diagrams`).
- **Which project:**
  1. The personal project, if set.
  2. Otherwise, if the person knows exactly one project, that one.
  3. Otherwise, a small popover: "New plan in" + a project select (personal, pinned, then recent) + **Create**.
  4. With no project at all, the action is shown but opens Get started at step 1, with the tooltip "Create a project first".
- **Exceptions:**
  - Workspace's action is **New project…**. It opens Workspace's template dialog (`?start=template`).
  - Study has a document type but no create form (agents build corpora), so it has no action. Its description says "Agents build study corpora with the Study tools."
  - Search, Knowledge base, Map and Opportunities have no action.
- **Where it shows:**
  - Guided rows: a link-styled button (`accent`, `meta`) under the description.
  - Compact rows: on hover or focus, at the right, in place of the chevron.
  - Every row's right-click menu: **Open** and **New …**.

### Footer

The existing note about connection details, in `meta`, `muted`, centred.

## First run

What the start page shows depends on how far the person has set up Atlas. It reads only the identity settings, which are already loaded, so the state is known at first paint.

| State | When | Left column |
|---|---|---|
| **First run** | No recent projects, no pins, no personal project | Get started; no Today, no Continue. The title reads "Welcome to Atlas" instead of the date |
| **Set up partly** | Some recent projects, but no pins and no personal project | Get started (step 1 done), then Continue. If Get started is hidden: one line, "Pin a project with ☆ to see its work here." and **All projects →** |
| **Normal** | A personal project or at least one pin | Today and Continue |

Get started stays until the page is Normal or the person hides it; a step that is done shows ✓ and "Done".

Tools use the guided density in the first two states and the compact density in Normal.

### Anatomy (First run, ≥1200px)

```
┌──────────────────────────────────────────────────────────────────────────────┐
│ [logo 40px]                                                         (PM) ▾   │
├──────────────────────────────────────────────────────────────────────────────┤
│ Welcome to Atlas                                                             │  title
│ ⌣ underline                                                                  │
│                                                                              │
│ GET STARTED                                            Hide │ TOOLS          │
│ ┌─────────────────────────────────────────────────────────┐ │ ▦ Planner    › │
│ │ 1  Create your first project                            │ │   Plans, sched…│
│ │    A project is a workspace: your files, plans, maps    │ │   New plan     │
│ │    and models live in it.                               │ │ ⟲ Model      › │
│ │    [New project from template…]   Open a shared project │ │   Cause and e… │
│ │ ─────────────────────────────────────────────────────── │ │   New model…  │
│ │ 2  Choose where your own TODOs live                     │ │ …              │
│ │    [select a project ▾]  Use as personal project        │ │ ✎ Study      › │
│ │ ─────────────────────────────────────────────────────── │ │   Agents build…│
│ │ 3  Pin the projects you work in                         │ │                │
│ │    Their work shows here and in My work. All projects → │ │                │
│ └─────────────────────────────────────────────────────────┘ │                │
└──────────────────────────────────────────────────────────────────────────────┘
```

### Get started

- **Card:** `surface`, a `border` hairline, `radius-lg`, padding `space-4`. Steps are separated by a hairline.
- **Heading row:** the eyebrow "Get started". At the right, **Hide** (a link). Hide is saved in the identity settings (app `atlas`, key `launcherSetupHidden`), so it holds on every device.
- **Each step:**
  - a 24px number disc (`surface-2`, `ui-strong`); when done, a ✓ in `success`, and the word "Done" after the title
  - the title (`ui-strong`)
  - one line of explanation (`ui`, `muted`)
  - its action
- **A step that is not reachable yet** shows its title and explanation in `muted`, with the action disabled. The action's tooltip says what to do first.
- **Step 1, Create your first project.**
  - Primary button **New project from template…** opens Workspace's template dialog (`?start=template`).
  - Link **Open a shared project** opens Workspace's Open dialog (`?start=open`).
  - Done when the person has a recent project.
- **Step 2, Choose where your own TODOs live.**
  - A select of the person's projects and the button **Use as personal project**. It saves in place, with no navigation; the personal project is also pinned.
  - Reachable after step 1. Done when a personal project is set.
- **Step 3, Pin the projects you work in.**
  - The explanation and the link **All projects →**.
  - Done when anything is pinned. Step 2 pins the personal project, so step 3 usually completes with it.
- **When step 2 or 3 is done,** the page is Normal and the card is gone (step 2 pins the personal project, so it completes step 3 too).

## Signed out: the cover

- Centred, max width 640px, padded 15vh at the top.
- The horizontal logo, 64px high.
- The headline "Industrial power. Bespoke thinking." in 32px, 700, `fg`, with the curved underline under it (96×10, `brand-highlight`).
- The canonical short description from the brand book (`body`, `muted`).
- The sign-in buttons: the first is the primary button (`accent`); "Use a different account" is a link.
- No Today, no Continue, no Tools.

## Loading and failure

This follows [ui_design.md]({{ site.baseurl }}{% link docs/developers/design/ui_design.md %}), "Loading and failure".

| Region | First paint | While fetching | Failure |
|---|---|---|---|
| Header, date | Final | – | – |
| Tools | From the cached server configuration; otherwise 6 skeleton rows and a hidden status "Loading your tools…" | Skeleton rows | One line "Your tools could not be loaded." with **Try again** |
| Today | The last counts and Next up rows cached in this browser, with "as of 10:12" in the heading row; otherwise skeleton tiles (numeral block 24×20) and 3 skeleton rows | The cached or skeleton content stays | One line "Your work could not be loaded." with **Try again**; tiles hidden |
| Continue | Names from the identity settings at once | Meta lines are a 120×12 skeleton | A project whose details fail shows only its name |

The Today fetch starts after first paint (idle, at most 1s later). It is **one** request for the personal and pinned projects together. Continue makes two requests per listed project, so at most 12.

## Empty states

One line each, in `muted`. A link is in `accent`.

- **Nothing pinned and no personal project:** "Pin a project with ☆ to see its work here." followed by the link **All projects →**.
- **Nothing of mine is open:** the tiles show zeros, and in place of Next up: "Nothing due today."
- **No recent projects:** Continue is not shown (in First run, Get started takes its place).
- **No tools:** "No tools are available for your account yet."

## Keys

- `g w` opens My work; `g p` opens All projects.
- Tab order: header, Today tiles, Next up rows, Continue rows, Tools.
- The suite's Ctrl+. (agent) and ⌘K keep working.

## Implementation

`packages/app-suite/src/`:
- `launcher.tsx`: the hash view switch, and the cover when signed out.
- `launcher/`: `LauncherHeader`, `GetStarted`, `TodayStrip`, `ContinueList`, `ToolList` (with the New action and its project popover), `AllProjects` (from `launcherProjects.tsx`).
- Workspace web (`apps/workspace/web/src/App.tsx`) accepts `?start=template` and `?start=open`, which open its existing template and Open dialogs.
- `AtlasMark.tsx`: the symbol as inline SVG.
- `launcher.css`: styles from tokens only.

Data comes from Workspace `POST /api/worklists` (the worklists and the "waiting on me" counts of a list of projects) and the existing `/head` and `/open-threads` calls for Continue.
