---
title: "UI design language"
parent: Design
grand_parent: Developers
nav_order: 2
---

# UI design language

This guide is the human-readable website edition of the [source specification](https://github.com/delzubu/atlas-core/blob/4a656981ba21c6139a3d3750e0cb76f8d706284a/docs/design/ui_design.md). It preserves the design rules and their identifiers; implementation status and measurements are those recorded in the source, not a fresh audit.

Atlas is a suite of working tools: kb-explorer, the workspace viewer, the map viewer, search and the launcher. Each one is a navigator over items plus a view of the item you picked. The design system holds one rule above the others: **content gets the screen; chrome is one line tall and everything else is one keystroke away.**

## Relation to the brand book

[product-brandbook.md]({{ site.baseurl }}{% link docs/developers/design/product-brandbook.md %}) defines the identity: what Atlas is, the logo and the brand palette (§12). This file turns that identity into the language the product UI is built with: tokens, layout, components and rules.

- **Flow and behaviour belong to [ux.md]({{ site.baseurl }}{% link docs/developers/design/ux.md %}).** It says how a person works with an app: the journey, one step per screen, connected workflows, references, agents, speed and devices. This file says how that looks and where things sit.
- **Each decision has one owner.** The brand book owns the logo files and the brand hexes. This file owns every token, and says for each token which brand colour it uses, or that it is UI-only.
- **Conflicts.** For the logo and the brand hexes, the brand book wins. For anything in the product UI, this file wins.
- **The scheme is deliberately conservative.** The product is a working surface: content carries the colour, chrome stays neutral. The brand's strong colours are used for identity, not for function. Publications and marketing use the brand book directly and may be louder.

## Principles

1. **One shell, four regions.** Every app uses the same frame: AppBar (`appbar-h`), Navigator (`panel-w`), Document (DocHeader `docheader-h` + body) and Inspector (`inspector-w`). An app may leave a region empty, but it never invents a new one. A floating card, a second toolbar or a fixed side column is a missing Inspector tab.
2. **Lean by default, full on demand. Frequent commands are one click away.** The visible surface shows only what a user needs in the current moment: the tree, the title, 2–3 primary actions. Everything else sits behind one of three doors, in this order:
   - the ⋯ overflow in DocHeader
   - an Inspector tab
   - the command palette (⌘K)

   Every action is reachable through the palette. That is the contract that lets the visible chrome shrink. But the *frequent* item commands (create, edit, delete) must not hide behind it: they sit as icon tools at the top of the navigator and in right-click menus on items (see Commands).
3. **Chrome is quiet, content is loud.** Chrome sits on `surface-2` and uses the `ui` style (13px). Content sits on `surface` and uses the `body` style (15px). Chrome never uses `accent` except for focus, the current selection and the single primary action.
4. **Regions collapse, they do not vanish.**
   - The navigator collapses to a `rail-w` icon rail, never to nothing.
   - The Inspector closes to a toggle in DocHeader that carries a count ("Related 5").
   - A user can always see that more exists.
5. **Same gesture, same place, every app.** `/` filters the navigator, `]` toggles the Inspector, `[` toggles the navigator and ⌘K opens the palette. A user who learns one Atlas app has learned them all.

## Layout

![Knowledge base desktop shell after the redesign]({{ site.baseurl }}{% link docs/developers/design/screenshots/kb-after-1920.png %})

The desktop example shows how the navigator, document and inspector share the available width. The [interactive-looking static shell preview]({{ site.baseurl }}{% link docs/developers/design/previews/Shell.html %}) illustrates the layout in isolation. The [component reference]({{ site.baseurl }}{% link docs/developers/design/components/index.md %}) covers each region in detail.

Design for the **laptop on a 1920×1080 screen first**. The browser viewport is about 1920×950. Tablet and phone must work but are not optimised. What "must work" means, and how an app that chooses to put the phone or tablet first does it, is [mobile.md]({{ site.baseurl }}{% link docs/developers/design/mobile.md %}) (the mobile layer: form factors, touch, typing, the Action bar, Sheet and FocusTask). Each app declares its target (`laptop`, `touch` or `phone-first`) in its `AGENTS.md`.

- **Primary (≥1600px):** `AppBar / [Navigator | Document | Inspector]`.
  - The Navigator is pinned at `panel-w` (320px), so titles fit on one line.
  - The Inspector is **docked and open by default** at `inspector-w` (360px). It sits directly against the text column, not at the far screen edge.
  - The Document text column is capped at `measure` (720px), left-aligned with the title.
  - At 1920px the free width is right margin, never line length.
- **Compact laptop (1200–1599px):** the Navigator stays pinned. The Inspector stays docked but is closed by default and remembers the user's choice.
- **Tablet and phone (<1200px), secondary** unless the app targets `touch` or `phone-first` ([mobile.md]({{ site.baseurl }}{% link docs/developers/design/mobile.md %}), which wins below 1200px):
  - The Navigator and Inspector become overlays with `shadow-overlay`.
  - The AppBar shows ☰.
  - DocHeader actions collapse into ⋯.
- **Vertical space is the scarce axis** at 16:9. The chrome budget is 76px in total: `appbar-h` plus `docheader-h`. Nothing else may sit above the text, and nothing floats over it, except what a person summons and can move or dismiss: Help's window ([Help]({{ site.baseurl }}{% link docs/developers/design/components/Help.md %})) and the agent's edge handle and window ([Assistant]({{ site.baseurl }}{% link docs/developers/design/components/Assistant.md %})). The agent's handle is a suite control, like Help's `?`: it belongs to no app.
- **Installed app:** a 34px tab strip sits above the AppBar (see [Tabs]({{ site.baseurl }}{% link docs/developers/design/components/Tabs.md %})), bringing the chrome to 110px. It is the one addition to the budget, and it only appears in the installed app, where the browser's own tab strip and address bar are gone.
- **No reserved offsets.** The command dock is off by default. The ⌘K field in the AppBar replaces it.

## Color

**One theme: black on white, like a printed page, in an ink frame.** The AppBar is `chrome-ink` with `on-chrome` text; everything below it is the page. There is no dark mode, no theme toggle, and no `prefers-color-scheme` switching. The system ignores the OS theme, so the page always reads the same.

**One function, one token, one colour.** Every colour in the UI comes from a token in `packages/app-suite/src/tokens.css`, and every token has exactly one job, listed below. Never use a token for another job because the colour looks right, and never write a hex in component CSS. If a job has no token, add one here, in `tokens.css` and in `tokens.json` in the same change.

### Neutrals (the brand's neutrals, exactly)

| Token | Hex | Brand name | Job |
|---|---|---|---|
| `bg`, `surface` | `#ffffff` | White | Page and content regions (navigator, document, Inspector, popups) |
| `surface-2` | `#f6f7f9` | Mist | Chrome only: DocHeader, the rail, tool tiles, skeletons |
| `border` | `#e2e2e2` | Border | Hairlines between regions and around inputs and cards. Regions are separated by a hairline, never by fill |
| `chrome-ink` | `#1f2328` | Graphite | The AppBar and the installed app's tab strip |
| `fg` | `#1f2328` | Graphite | All primary text (15.8:1 on white). Rows on `selected` use `fg` for every text, dates included |
| `muted` | `#6b7280` | (UI-only) | Secondary text on white or `surface-2` only |
| `hover`, `selected` | `#eef0f2`, `#dbeafe` | (UI-only) | Row and button hover; the current selection |
| `on-chrome`, `on-chrome-muted`, `chrome-hover`, `chrome-line` | | (UI-only) | Text, hover and lines on `chrome-ink` |

<div style="display:flex;flex-wrap:wrap;gap:8px;margin:16px 0;"><span style="padding:12px;border:1px solid #e2e2e2;border-radius:4px;background:#fff;color:#1f2328">White</span><span style="padding:12px;border:1px solid #e2e2e2;border-radius:4px;background:#f6f7f9;color:#1f2328">Mist</span><span style="padding:12px;border:1px solid #e2e2e2;border-radius:4px;background:#1f2328;color:#fff">Graphite</span><span style="padding:12px;border:1px solid #e2e2e2;border-radius:4px;background:#dbeafe;color:#1f2328">Selected</span><span style="padding:12px;border:1px solid #e2e2e2;border-radius:4px;background:#2563eb;color:#fff">Interaction</span></div>

### Identity (the brand's colours, restricted)

| Token | Hex | Brand name | Job |
|---|---|---|---|
| `brand` | `#5b21b6` | Aubergine | The Atlas mark; section eyebrows on the launcher; the signed-out cover headline |
| `brand-highlight` | `#f97366` | Coral | The curved-underline motif, at most once per screen. Decorative only (2.7:1 on white): never text |

Identity colours are **never** used for links, buttons, focus, selection, status or data. Aubergine on `chrome-ink` is 1.8:1, so on the AppBar and the tab strip the mark sits on a white `radius-md` tile.

**Where identity colour may appear**, and nowhere else:
- the launcher's start page and its signed-out cover
- the Atlas mark in the AppBar and the tab strip
- the installed app's icons and favicon (the files in `packages/app-suite/public/icons/`)

Every other screen uses neutrals, interaction, status and data colours only.

### Interaction (UI-only)

| Token | Hex | Job |
|---|---|---|
| `accent` | `#2563eb` | Links, the focus ring (2px solid, 2px offset), the active Inspector tab underline, and the one primary button per view. In chrome, `accent` is used for nothing else |
| `on-accent` | `#ffffff` | Text and icons on an `accent` fill |

The launcher uses `accent` like every other screen: a primary button, a link and focus look the same everywhere in Atlas.

### Status (UI-only, always with a word or glyph)

| Token | Hex | Job |
|---|---|---|
| `danger` | `#b91c1c` | Errors, destructive actions, **overdue** |
| `warning-border`, `warning-bg` | `#f59e0b`, `#fef3c7` | **Due today**; the unsaved dot (labelled "unsaved" in its tooltip) |
| `success` | `#16a34a` | Done, saved |

Colour is never the only signal: ⚠, ✓, a dot plus a word ("Today"), or a label always comes with it.

### Data (UI-only)

- **Item-type icon tints** (`icon-*`): see Iconography.
- **Highlights** (kb timeline): `hl-1` … `hl-6` colour user-defined highlight rules. Each is ≥ 5:1 on white and used as a border and dot over a 14–16% tint, so labels stay `fg`. A legend always names them, with counts, so colour is never the only signal. At most six at a time.
- **An app's own data colours** (a pipeline's groups, a person's working style, a standing) are that app's tokens, a `:root`
  block in its own stylesheet, never in the shared `tokens.css`. The same rules hold: each is ≥ 4.5:1 on white, used as a border,
  dot or word over a light tint of the same colour, always beside its word or number, and never a status colour.
- **Items an app draws** may keep their own colour dot: see Iconography.

## Type

- System sans (`sans`) throughout; `mono` for paths and source mode.
- Chrome uses exactly two styles:
  - `ui` (13/18) for rows, menus and buttons, with `ui-strong` for headings inside chrome
  - `meta` (12/16) for trailing badges and chips
- `caption` (11/14) is only for counts and keyboard hints.
- Documents use `body` (15/24), `heading` (18/24) and `title` (20/26).
- The title lives **in DocHeader**, not as a 32px H1 in the body, which is the largest single vertical saving after the AppBar change.
- Sentence case everywhere: "Copy link", "Add relation", "Open on GitHub". No Title Case, no trailing periods on labels.

## Spacing, size and density

- **Spacing:** the scale is `space-1`…`space-5` (4/8/12/16/24).
  - Regions pad `space-3` horizontally.
  - Documents pad `space-5` on wide screens and `space-4` on narrow ones.
- **Rows:** tree rows are `row-compact` (24px) by default on a fine pointer and `row-default` (28px) in comfortable density. On a coarse pointer the minimum is `row-touch` (44px).
  - Density is a user preference stored with the suite preferences, like panel width.
- **Radius:** use `radius-sm` for chips and row fills, `radius-md` for buttons and inputs, and `radius-lg` for popups and cards. Nothing else: the 7/10/12px radii in the current chrome fold into `radius-lg`.
- **Elevation:** only floating things cast shadows: `shadow-popup` for menus and the palette, `shadow-overlay` for overlaid panels, `shadow-float` for the dock and the task pill. Docked regions are flat.

## Iconography

One outline icon set, 16px on a 1.5px stroke, drawn as inline SVG.

- **Item-type icons are tinted by type** (`TreeIcon`, `--icon-*` tokens). The same set is used in every app:
  - folders amber, with a light fill
  - Markdown blue
  - images green
  - data/config violet
  - code slate
  - map layers teal
  - anything else grey
- Each tint is ≥ 3:1 on white and on `selected`.
- Map items keep their own colour dot. That dot is data, not an icon.
- Tools and actions use the icons in `fg`, in the muted colour when disabled.
- Emoji are never icons. Twisties are chevrons that rotate.
- Every icon-only button has an `aria-label`, and a tooltip that includes its shortcut ("Toggle Inspector  ]").

## Commands: where they live

Each command has one home that is always visible, plus the shared ways in:

1. **Navigator tools**: an icon strip under the navigator heading, also shown in the rail when the navigator is closed.
   - Create first (Add POI/path/region; New file/folder, Upload), then a divider.
   - Then the selected item's commands (Edit vertices, Append points, Labels, Delete; Rename, Download, Delete).
   - A disabled tool says why in its tooltip.
2. **Right-click** on an item: in the tree, on the map, and via Shift+F10 or the menu key on the tree's cursor row. It shows that item's commands.
3. **DocHeader**: the open item's primary actions, and ⋯ for the rest.
4. **The palette (⌘K)**: everything.

Menus and popovers float above the page (portalled and kept inside the viewport). A wrapped header or a scrolling pane never clips them.

## Loading and failure

- **Content of a known shape** (rows, tiles, counts): skeletons in its place, blocks of `surface-2` at the size of what will come. They are static under `prefers-reduced-motion`. A visually hidden `role="status"` names what is loading ("Loading your tools…").
- **One action of unknown length** (signing in, a save): the spinner, next to the thing it is for.
- **Data that can be shown from a cache**: show the cached copy at once, marked "as of 10:12" in `meta`, and refresh it in the background. Never blank a screen that has something to show.
- **A failure** affects only its region: one line there, saying what failed, with **Try again**. The rest of the screen keeps working.

## Voice

Terse, literal and verb-first. Buttons name the action ("Save", "Revert", "Add relation"), never the state. Counts are numerals ("261 events", "Related 5"). Empty states say what to do next in one line ("No related events. Press + to add one."). Never "Oops".

## Keyboard map

| Key | Action | Scope |
| --- | --- | --- |
| ⌘K / Ctrl+K | Command palette | all apps |
| Ctrl+. / ⌘+. | Open or close the agent | all apps ([Assistant]({{ site.baseurl }}{% link docs/developers/design/components/Assistant.md %})) |
| `/` | Focus navigator filter | all apps |
| `[` | Collapse / expand navigator to rail | all apps |
| `]` | Toggle Inspector | all apps |
| `1`…`9` with focus in the Inspector | Switch Inspector tab | all apps |
| ↑ ↓ ← → Home End PgUp PgDn Enter, letters | Tree navigation, type-ahead | Navigator |
| Shift+F10 / menu key | Right-click menu for the cursor row | Navigator |
| F2 | Rename (workspace: dialog) | workspace, edition apps |
| E | Edit document | kb, workspace, edition document apps |
| N | New article | kb |
| R | Reload the document (keeps the view) | planner, edition document apps |
| E | Edit mode (view mode is the default) | planner |
| Insert | New task after the selection | planner |
| Alt+Shift+→ / Alt+Shift+← | Indent / outdent the selected items | planner |
| L | Link the selected items one after another | planner |
| Ctrl+Z / Ctrl+Y | Undo / redo this editor's edits | planner, edition document apps |
| Z / T | Zoom the chart to fit the plan / go to today | planner |
| + / − | Zoom the chart | planner |
| . | Reveal the selection (scroll and zoom the chart to it) | planner |
| Alt+← / Alt+→ | Back / forward (browser history) | kb, workspace |
| T | Timeline band | kb |
| ⌘S | Save | workspace |
| Ctrl+PageDown / Ctrl+PageUp | Next / previous tab | installed app ([Tabs]({{ site.baseurl }}{% link docs/developers/design/components/Tabs.md %})) |
| Ctrl+1 / Ctrl+2…8 / Ctrl+9 | Start tab / tab 1…7 / last tab | installed app |
| Alt+W / Alt+Home | Close tab / start tab | installed app |
| Middle-click, Ctrl+click an item | Open in new tab | kb (workspace: middle-click; Ctrl+click is multi-select) |
