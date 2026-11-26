---
title: MyWork
parent: Component reference
grand_parent: Design
nav_order: 16
nav_exclude: true
---

# My work

[Component reference]({{ site.baseurl }}{% link docs/developers/design/components/index.md %}) · [Source specification](https://github.com/delzubu/atlas-core/blob/4a656981ba21c6139a3d3750e0cb76f8d706284a/docs/design/components/MyWork.md)

My work lists every open TODO and plan item from the person's **personal and pinned projects**. It answers "what is mine to do?" across all of them, and lets them act on an item without opening Planner. It lives at `/apps/atlas/#work` and is reached from the launcher's Today strip.

It is a working screen, so it has the shell's three regions (Navigator, Document, Inspector; [ui_design.md]({{ site.baseurl }}{% link docs/developers/design/ui_design.md %})) and **no identity colour** in them. It stays under the launcher's header rather than the ink AppBar: it is a view of the launcher (`#work`), not an app, and the suite shell's state (preferences, help, Look up, the agent's context) is keyed by app.

## Which items, in which list

The items come from the plans' worklists (`docs/specs/document-worklist.md`, v2). For the signed-in email, each item belongs to at most one view:

| View | An item is here when |
|---|---|
| **Mine** (default) | I am among `assignees`; or nobody is assigned and I am the `owner` |
| **Waiting on others** | I am the `owner`, others are assigned, and I am not one of them |
| **Up for grabs** | Nobody is assigned and there is no owner: it is everyone's |

Items not yet started (`start` after today) are left out of every view. A repeating item shows its current occurrence.

## Anatomy (≥1600px)

```
Launcher header 56  [logo]                                                           (PM)
┌ Navigator 320 ─────────┬ DocHeader 40 ──────────────────────────────────────────────────┬ Inspector 360 ──────┐
│ ⟳                      │ My work  [Mine 18 | Waiting on others 4 | Up for grabs 17]  ⋯  │ Call the bank       │
│ All pinned         18  │ Kind: All ▾   Group: Urgency ▾                                 │ home › My TODOs ↗   │
│ ★ home (personal)   6  │ ▾ Overdue 3                                                    │ New · High          │
│ ★ q4-launch         7  │   ○ Call the bank      home › My TODOs     ▲ High  ⚠ 20 Oct    │ Owner: you          │
│ ★ garden            2  │   ◐ API review         q4-launch › Plan   ▬▬ 40%   ⚠ 28 Oct    │ Due 20 Oct          │
│ ★ site-x            ⚠  │ ▾ Due today 5                                                  │ ─────────────────── │
│                        │ ▸ This week 6                                                  │ About the transfer  │
│                        │ ▸ Later 9                                                      │ limit.              │
│                        │ ▸ No date 4                                                    │                     │
│ All projects →         │ Not included (3) ▸                                             │ [Done]              │
└────────────────────────┴────────────────────────────────────────────────────────────────┴ Progress 0 25 50 75 100
                                                                                           ETA [date]  Open in Planner ↗
```

### Header

- The launcher's header (logo, account menu). The navigator's **← Start** returns to the start page.
- The DocHeader says "Updated 10:12" or "Updating…"; a failed refresh is a line in the list with **Try again**.

### Navigator: the scope

- Heading "Projects". The tool strip holds **Refresh** (`r`).
- **All pinned**, then the personal project (marked "personal"), then the pinned projects in pin order.
- Each row shows the count of the current view's items (`meta`, trailing). The row's state is:
  - loading: a skeleton count
  - out of date (any of its lists is `stale`): "out of date" in `muted`
  - unavailable: ⚠ with a tooltip, "This project cannot be read (access withdrawn or deleted)"
- Selecting a row scopes the list to that project. `/` filters the rows.
- Right-click on a project row: **Open in Workspace**, **Unpin**, **Set as personal project**.
- The footer link **All projects →** opens `#projects`, where projects are pinned.

### DocHeader

- Title "My work".
- A segmented control with the three views, each with its count.
- Two menus:
  - **Kind**: All, TODOs, Tasks.
  - **Group**: Urgency, Project. Mine and Waiting default to Urgency; Up for grabs defaults to Project.
- The ⋯ menu: **Refresh**, **Expand all groups**, **Collapse all groups**.

### List

- **Group headers:** a 28px row with a chevron, the name (`ui-strong`) and the count (`meta`, `muted`). Collapsing a group is remembered per view.
- **Urgency groups**, in order:
  - Overdue (`due` before today)
  - Due today
  - This week (due within 7 days)
  - Later
  - No date
- **Project groups:** `project › plan`, in the navigator's order.
- **Item rows** (`row-default`, 28px), left to right:
  - **Status glyph:** ○ new, ◐ in progress, ⏸ waiting, ↻ a repeating item's occurrence.
  - **Title** (`ui`).
  - **Where:** `project › plan` (`meta`, `muted`); left out when grouped by project.
  - **Priority:** "▲ High" or "▲▲ Urgent" (`meta`); nothing for normal and low.
  - **Progress**, for tasks: a 40×4px bar (`accent` on `border`) and the percentage.
  - **Due chip:** "⚠ 20 Oct" in `danger` when overdue; a `warning-border` dot and "Today" when due today; otherwise the date in `muted`.
  - In Waiting on others, the assignees' names replace the priority.
- **Selection:** the selected row uses `selected`. Its details show in the Inspector.
- **Not included (n):** a collapsed footer listing what took no part, each with its reason:
  - "site-x: cannot be read"
  - "Roadmap (q4-launch): no list (fictional calendar)"
  - "Plan (home): out of date, as of an earlier version"
  - "Plan (q4-launch): only the first 500 open items"

### Inspector: the selected item

- The title (`heading`) and a link to its plan (`AtlasLink`, `project › plan ↗`).
- Facts in `meta` rows: status, priority, owner, assignees, start, due, ETA, and the current occurrence for a repeating item.
- The description, as Markdown (as the Workspace worklist panel renders it).
- Actions: only those the item's `actions` allow.
  - **Done**: the view's one primary button.
  - **Progress**: 0 / 25 / 50 / 75 / 100, a segmented control.
  - **ETA**: a `DateField`.
- **Open in Planner ↗**: the item's `atlas://` link.
- Without a selection: "Select an item to see its details."

Responsive behaviour is that of the shell: docked open at ≥1600px, docked but closed by default at 1200–1599px (remembered), an overlay below 1200px.

## Acting

- An action is sent to the plan's app (Planner's worklist route).
- **Done:** the row is struck through at once and leaves the list after 1.5s, and the counts update. That one project is then fetched again.
- **Progress or ETA:** the row updates in place.
- **A refusal** ("That item is not assigned to you", a conflict, no access) puts the row back and shows a toast with the message.
- There is no undo: the route cannot reopen an item. Done asks for no confirmation.

## Keys

| Key | Action |
|---|---|
| ↑ ↓, j k | Move the selection |
| Enter | Open the Inspector on the selection |
| x | Done |
| o | Open in Planner |
| r | Refresh |
| 1 2 3 | Mine, Waiting on others, Up for grabs |
| `/` `[` `]` | Filter projects, toggle the navigator, toggle the Inspector (as in every app) |

## URL

`#work?view=mine|waiting|grabs&scope=<project>/<ref>&kind=all|todo|task&group=urgency|project&show=overdue|today|underway&item=<atlas uri>`

- Every part is optional.
- The launcher's tiles link to `show=` and its Next up rows link to `item=`.
- The view, kind and group last used are remembered in this browser and used when the URL does not name them.

## Loading and failure

This follows [ui_design.md]({{ site.baseurl }}{% link docs/developers/design/ui_design.md %}), "Loading and failure".

- **First paint:** the navigator lists the projects from the identity settings at once, with skeleton counts. The list shows the last response cached in this tab (with "as of"), or 8 skeleton rows.
- **One request** (`POST /api/worklists`) brings every project. A project that fails is marked in the navigator and listed under "Not included". The others show normally.
- **Whole request fails:** one line in the list, "Your work could not be loaded.", with **Try again**.

## Empty states

- **Nothing pinned and no personal project:** "Pin a project with ☆ to see its work here." followed by the link **All projects →**.
- **Mine is empty:** "Nothing is assigned to you. Up for grabs: 17." The count links to that view.
- **Waiting on others is empty:** "Nobody is working on something you own."
- **Up for grabs is empty:** "Nothing is up for grabs."

## Implementation

`packages/app-suite/src/mywork/`:
- `MyWork.tsx`: the shell and the URL state.
- `WorkList.tsx`, `WorkInspector.tsx`.
- `useMyWork.ts`: the fetch, the cache and the actions.

The classification and the urgency groups are pure functions in `worklist/classify.ts`, tested against `docs/specs/document-worklist.cases.json`.
