---
title: Help
parent: Component reference
grand_parent: Design
nav_order: 10
nav_exclude: true
---

# Help

[Component reference]({{ site.baseurl }}{% link docs/developers/design/components/index.md %}) · [Source specification](https://github.com/delzubu/atlas-core/blob/4a656981ba21c6139a3d3750e0cb76f8d706284a/docs/design/components/Help.md)

Help is the suite's one place for reading documentation inside an app. It is a non-modal window that a person can move,
resize, and dock to the right edge, so they can read while they work. It follows its own links. Every app gets the same
window from `app-suite` (`src/help/`); each app ships its own pages, written in Markdown, next to its build
(`<app>/help/`, built into `dist/help/` by the `atlasHelp` Vite plugin). Preview: [previews/Help.html]({{ site.baseurl }}{% link docs/developers/design/previews/Help.html %}).
Plan: [2026-10-18-suite-help.md](https://github.com/delzubu/atlas-core/blob/4a656981ba21c6139a3d3750e0cb76f8d706284a/docs/plans/2026-10-18-suite-help.md). **Status: implemented**.

## Entry points
1. **AppBar help button** (`?` icon, 28px, `on-chrome-muted`, `on-chrome` on hover). It sits in the trailing slot,
   before the account button. It is a suite control like the account button, so the "no app-specific buttons in the
   AppBar" rule still holds. It opens the page for **what the person is looking at** (the app's current topic, below),
   or the app's start page.
2. **F1**, and `?` when the focus is not in a text field. The same as the button. Pressing it again while Help has
   the focus closes it.
3. **Inline help links**: a small `?` after a field label, an Inspector section heading or a dialog title
   (`<HelpLink topic="concepts/links#polarity" />`). It opens Help at that heading. Use one where a field changes the
   output in a way its label can't say ("Timing", "Strength", "Kind").
4. **The command palette** lists help pages as `Help: <page title>` rows (group Help), found by title and headings.

## Anatomy (floating)
```
┌ ⠿  ←  →   Model help › Concepts › Links           ⌕  ⇥  ×  ┐   title bar 32px, surface-2
│ Contents ▾                                                 │   collapsible table of contents
│                                                            │
│  ## Polarity                                               │   page, body 14/22, measure 64ch
│  If the source rises, all else equal …                     │
│                                                            ◢   resize handle (and all edges)
└────────────────────────────────────────────────────────────┘
```
- **Title bar**: grip (⠿; the whole bar drags), Back and Forward, the breadcrumb of the page (app help › section ›
  page; each crumb a link), search (⌕), dock or float (⇥ / ⧉), close (×).
- **Screen bar** (only when needed): under the title bar, "Help for this screen: **Comments** — Show" appears when the
  open page is not the one for what is on screen (the reader followed links, or the screen changed under an open
  window). The window never changes page on its own; Show goes to the screen's page, and the first crumb always goes to
  the app's main help.
- **Contents**: the app's page tree from its `index.md`, collapsed by default in floating mode and shown as a left
  column when the window is wider than 720px.
- **Page**: rendered Markdown (headings, paragraphs, lists, GFM tables, code, images, block quotes as notes). Each
  heading has an anchor and a hover "copy link".
- **Footer** (only when the page has one): "Related" links from the page's front matter.

## States
| State | Where | Size | Notes |
| --- | --- | --- | --- |
| Closed | – | – | Default |
| Floating | Over the document, `z-overlay-panel`, `shadow-overlay`, `radius-lg` | Default 480×640, min 320×240, clamped to the viewport | Default position: right edge, under the AppBar, clear of the Inspector |
| Docked right | A grid column at the far right of `.atlas-frame` (after the Inspector), flat, `border-left` | Width 320–720, default 440; the edge drags | The document gives up the width; nothing is covered |
| Sheet (< 1200px) | Full-height overlay from the right; full screen on phone | – | Not draggable; docking is not offered |

- **Docking**: drag the window until the pointer reaches the right edge; a 4px `accent` drop line shows where it will
  dock. The ⇥ button, and the palette commands "Dock help" and "Float help", do the same without dragging (a click
  alternative is required, as for the command dock).
- **Persistence**: the state, position, size and dock width are a suite preference (`readStored`/`writeStored`,
  one per user, not per app), so Help reopens where it was. The last page per app is remembered for the session.
- **Clamping**: on resize of the browser the window stays inside the viewport and below the AppBar; it never covers
  the AppBar.

## Help and task guides

In-app help describes the current screen. Name its panels, fields and commands, explain where they are and what they do, and describe the states a reader may see. Publish end-to-end use cases in the matching app guide on the Atlas documentation site. Link to the guide from the relevant help topic and retain screen-specific topic paths used by F1 and inline help.

## Links inside a page

| Written in Markdown | Opens |
| --- | --- |
| `concepts.md#polarity`, `../modeling/index.md` | Another page of this app's help, in the window (history entry) |
| `#polarity` | A heading on this page |
| `suite:keyboard.md` | A suite page (shell, sign-in, links, keyboard), shipped with every app |
| `help:planner/plans.md` | Another app's help, resolved through the app registry (`APPS`); in the window |
| `atlas://…` | The object, through the suite's link handling (`AtlasLink`), in the app that opens it |
| `command:model.addLink` | Runs or highlights that palette command; shown as a button-styled link; disabled with a reason when the command is unavailable |
| `https://…` | A new browser tab, with an ↗ mark |

A link to a missing page or heading fails the app's build (see the plan); at run time it shows "This page is not in
this version's help" inside the window, never a browser 404.

## Keyboard
| Key | Action |
| --- | --- |
| F1, `?` | Open Help at the current topic; close it when Help has the focus |
| Alt+← / Alt+→ inside Help | Back / forward in Help's own history (not the browser's) |
| Ctrl+F inside Help | Search help (the ⌕ field) |
| Esc inside Help | Close Help and return the focus to where it was |

## Accessibility
- `role="dialog"`, `aria-modal="false"`, labelled by the title bar; it does not trap the focus, and Tab leaves it
  back into the app. Opening it moves the focus to the page (or to the heading a link names).
- Moving and resizing have keyboard and click alternatives (dock/float buttons and commands; "Reset help window" in
  the palette).
- With reduced motion, docking does not animate.

## API (`@atlas/app-suite`)
- The **button and window appear only when the app has pages**: the `atlasHelp()` Vite plugin defines
  `import.meta.env.VITE_ATLAS_HELP` when `<app>/help/` exists. `SuiteProvider help={{ base: 'help/' }}` forces it on
  and sets where the pages are (tests, embedding).
- `useSuiteHelpTopic('analysis/loops')`: the topic for what is on screen, while the component is mounted (like
  `useSuiteBreadcrumb`). The most recently mounted topic wins: the rank is taken in render order (parents before children), so a
  dialog inside a screen outranks the screen, however its effects ran or whichever topic changed later.
- `<HelpLink topic="concepts/links#timing" label="About timing" />`: the inline `?`.
- `openHelp(topic?)` from `useSuite()`: for "Learn more" buttons and empty states.
- `registerHelpBlock('model-example', Component)`: renders a fenced code block of that language as a component
  (an app's live example; a model app's example block runs the ops through its `sandbox` action and draws loops, patterns and
  what-if). Unregistered languages render as code.
- A page opens in another app with `?help=<page>#<anchor>` on its address (what `help:<app>/<page>` links use).

## Replaces
Ad hoc help text in empty states and dialog captions where it explains concepts at length; short captions stay.
