---
title: Sheet
parent: Component reference
grand_parent: Design
nav_order: 17
nav_exclude: true
---

# Sheet

[Component reference]({{ site.baseurl }}{% link docs/developers/design/components/index.md %}) · [Source specification](https://github.com/delzubu/atlas-core/blob/4a656981ba21c6139a3d3750e0cb76f8d706284a/docs/design/components/Sheet.md)

A panel that slides over the page from the bottom or from a side, above a scrim. It is how the Inspector, the Navigator and modal dialogs
appear on small screens ([mobile.md]({{ site.baseurl }}{% link docs/developers/design/mobile.md %}), Regions). Help and the agent window already behave this way on a phone
([Help]({{ site.baseurl }}{% link docs/developers/design/components/Help.md %}), [Assistant]({{ site.baseurl }}{% link docs/developers/design/components/Assistant.md %})); this spec makes it one shared component. **Status: implemented** (`packages/app-suite/src/mobile/`, #220).

## Forms
| Form | Where | Size |
|---|---|---|
| Bottom sheet | the Inspector on a phone or a portrait tablet; short choices (Sort, Filter, a small form) | auto height up to 85 dvh; top corners `radius-lg`; a grab handle |
| Side sheet | the Navigator (left) on phones and tablets; the Inspector (right) on a landscape tablet | full height, up to 85 % width (max `panel-w` or `inspector-w` on a tablet) |
| Full-screen sheet | `SuiteModal` content on a phone; Help; the agent | the whole viewport inside the safe areas |

## Anatomy (bottom sheet)
```
          ───                         grab handle (decorative)
 Title                           ×    40px header: title (ui-strong), close (44px target)
 ─────────────────────────────────
 content (scrolls inside the sheet)
 ─────────────────────────────────
 [ Cancel ]              [ Apply ]    optional footer: same rules as ActionBar, plus safe-bottom
```

## Rules
- **Ground.** `surface` on a scrim (`overlay`), `shadow-overlay`. It slides in 200 ms; there is no motion under `prefers-reduced-motion`.
- **Closing.** It closes with ×, Escape, a tap on the scrim (unless it holds unsaved input; then the scrim does nothing, and × asks), and the
  browser's back (opening a sheet adds a history entry, so the system back gesture closes it instead of leaving the screen).
  Dragging the handle down closes a bottom sheet. This is an extra way, never the only one.
- **Focus.** On open, focus moves to the sheet's first control, or its title. It is trapped inside while open and returns to the opener on close.
  `role="dialog"`, `aria-modal="true"`, labelled by its title.
- **Scrolling.** Only the content scrolls. The page under it does not (`overscroll-behavior: contain`, body scroll locked).
- **Keyboard.** With the keyboard open, a sheet with a field shrinks to the visual viewport. Its footer stays above the keyboard.
- **Nesting.** One sheet at a time. A sheet that needs another closes itself first, or uses a step inside itself.

## What the consumer provides
```ts
<Sheet open title side="bottom" | "left" | "right" | "full" onClose footer?={ReactNode}>{content}</Sheet>
```
`SuiteModal` renders a full-screen Sheet below 768 px and stays a centred dialog above, so apps get it without changes. `SuiteFrame`
renders the Inspector and the Navigator as sheets below 1200 px.
