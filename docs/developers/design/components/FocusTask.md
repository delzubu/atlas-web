---
title: FocusTask
parent: Component reference
grand_parent: Design
nav_order: 7
nav_exclude: true
---

# FocusTask

[Component reference]({{ site.baseurl }}{% link docs/developers/design/components/index.md %}) · [Source specification](https://github.com/delzubu/atlas-core/blob/4a656981ba21c6139a3d3750e0cb76f8d706284a/docs/design/components/FocusTask.md)

The layout for working through items one at a time: answering practice questions, taking a test, a quick capture. It works on a phone first
and widens to a laptop. It is part of the mobile layer ([mobile.md]({{ site.baseurl }}{% link docs/developers/design/mobile.md %})). **Status: implemented** (`packages/app-suite/src/mobile/`, #220). Its first user was
an edition's practice screen (#220).

## Anatomy

Phone (390×844, portrait):
```
┌ AppBar (40) ─────────────────────────────┐
│ ×  Practice · Lessons 1–4     3/12   ⋯   │ DocHeader (40): close, title, progress, ⋯
│ ▓▓▓▓░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │ progress line (4px, accent, decorative; the words are "3/12")
│ 🐧 Take your time.                       │ mascot strip (≤56), optional, hideable
│                                          │
│ Translate                                │ format label (meta, muted)
│ the girl                                 │ prompt (heading 18px, wraps, never truncated)
│                                          │
│ ┌──────────────────────────────────────┐ │ answer input: full width, text-input-touch (16px)
│ │ das Mädchen                          │ │
│ └──────────────────────────────────────┘ │
│ [ä] [ö] [ü] [ß] [Ä] [Ö] [Ü]              │ character bar (44px keys), only while the field has focus
│                                          │
│ ✓ Correct                                │ feedback (after Check): word + glyph, then correction
├──────────────────────────────────────────┤
│ 3 of 12              [ Skip ]  [ Check ] │ ActionBar (56 + safe-bottom)
└──────────────────────────────────────────┘
```
Laptop (≥ 1200 px): the normal shell. The navigator stays (it may collapse to the rail), and the work area is the `measure` column. The mascot gets a sticky rail
beside it when the content is ≥ 1150 px wide, as in Study. The ActionBar's actions render as a button row under the answer, and the progress shows in
the DocHeader.

## Rules
- **One item per screen,** in a scrolling work area between the pinned header and the pinned actions. Only the work area scrolls.
- **The DocHeader on a phone:** a close control (×, which leaves and keeps the progress; it asks only if leaving would lose
  something), the task title, progress as words ("3/12"), and ⋯ (flag, skip, hide the mascot, help). The ☰ navigator is not offered
  on a focus screen.
- **Progress** is "3/12" in words, plus an optional 4 px decorative line (`aria-hidden`). Colour is never the only signal.
- **Answer controls** follow `mobile.md`: full-width option rows ≥ 44 px, the typing attributes table, the character bar, Enter = the
  primary action.
- **Feedback** (when the task checks each item) appears **below the answer, in the work area**, never in a toast or a dialog:
  - the verdict as a word plus a glyph and a status colour (`success` with ✓ "Correct", `warning-border` with ◐ "Partly right",
    `danger` with ✗ "Not right")
  - then the correction (the right answer, what to fix, accepted alternatives, the explanation)
  - The primary action becomes "Next" and gets the focus. On a phone the work area scrolls so the verdict's first line is visible.
- **Moving between items:** Next and, if the task allows, Back. A number grid ("Go to question…") lives in ⋯ on a phone and as a bar on a
  laptop. Swiping between items is not used (it conflicts with the system back gesture).
- **The end of the task** leads to a summary screen, which is not a FocusTask. It uses the normal shell and the mascot scene.
- **Leaving and returning:** answers are saved as the person goes (debounced), so a phone that locks or a tab that is closed resumes where it left off.
- **The mascot** ([Mascot]({{ site.baseurl }}{% link docs/developers/design/components/Mascot.md %})): a strip on a phone and a rail on a wide laptop, never a scene. It never reacts to correctness while the task
  is open, unless the app checks each item and the verdict has already been shown.

## What the consumer provides
```ts
<FocusTask title progress={{ n, total }} onClose menu={Action[]} mascot?={ReactNode}
  actions={{ status?, secondary?, primary }}>{item}</FocusTask>
```
It lives in `packages/app-suite/src/`. It composes `DocHeader`, `ActionBar` and the shell, and owns the pinned or scrolling split and the keyboard
handling (`visualViewport`).
