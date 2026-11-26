---
title: ActionBar
parent: Component reference
grand_parent: Design
nav_order: 1
nav_exclude: true
---

# ActionBar

[Component reference]({{ site.baseurl }}{% link docs/developers/design/components/index.md %}) · [Source specification](https://github.com/delzubu/atlas-core/blob/4a656981ba21c6139a3d3750e0cb76f8d706284a/docs/design/components/ActionBar.md)

The bar at the bottom of a phone screen that holds the screen's primary action, where the thumb reaches. It is part of the mobile layer
([mobile.md]({{ site.baseurl }}{% link docs/developers/design/mobile.md %})). On a laptop the same action stays in the [DocHeader]({{ site.baseurl }}{% link docs/developers/design/components/DocHeader.md %}), and the bar is not drawn.
**Status: implemented** (`packages/app-suite/src/mobile/`, #220). Its first user is Languages (#106, #220).

## Anatomy
```
┌──────────────────────────────────────────────┐
│  3 of 12 · saved          [ Skip ]  [ Check ]│  actionbar-h (56px) + safe-bottom
└──────────────────────────────────────────────┘
```
- **Leading status** (optional, `meta`, `muted`): progress or save state ("3 of 12", "Saved", "Saving…"). It is one line and never a control.
- **Secondary action** (optional, at most one): a quiet `ui` button.
- **Primary action** (exactly one): the `accent` fill and `on-accent` text, at least 44 px tall and at least 96 px wide. This is the screen's one accent
  button (`ui_design.md`: one primary per view). The DocHeader then shows no primary action of its own.

## Rules
- **Placement.** It is sticky at the bottom of the Document region, on `surface` with a top `border` hairline, padded
  `space-2` `space-4`, plus `padding-bottom: var(--safe-bottom)`.
- **Scrolling.** It is part of the layout: the content scrolls above it and is never covered by it.
- **With the on-screen keyboard open,** it rides on top of the keyboard. Its bottom follows `visualViewport` (height and
  offsetTop), so the action that submits the focused field stays visible.
- **When it shows.** Only below 768 px (`mobile.md`, Form factors), and on a tablet only for [FocusTask]({{ site.baseurl }}{% link docs/developers/design/components/FocusTask.md %}) screens. On a laptop its actions render in
  the DocHeader (or, on a FocusTask screen, under the work area as a normal button row) with the same labels and order.
- **Labels** follow the voice of `ui_design.md`: verb first, naming the action ("Check", "Next", "Start practice"). A disabled primary action
  says why in the leading status ("Choose an answer").
- **Toasts** appear above it. Nothing else may float over it.
- **Keyboard:** Enter in the screen's single field triggers the primary action. The bar's buttons are in the tab order after the
  content.

## What the consumer provides
```ts
<ActionBar status?={ReactNode} secondary?={Action} primary={Action} />   // Action = { label, onRun, disabled?: string /* reason */, busy?: boolean }
```
It lives in `packages/app-suite/src/` next to `DocHeader`, and is exported. On a laptop it renders nothing, and the app passes the same actions to
DocHeader. A hook `useActionPlacement()` returns `'bar' | 'header'`, so the app does not repeat the breakpoint.
