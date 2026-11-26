---
title: Forms
parent: Component reference
grand_parent: Design
nav_order: 8
nav_exclude: true
---

# Forms: Combobox, chips, DateField, Toasts, FieldConflictDialog

[Component reference]({{ site.baseurl }}{% link docs/developers/design/components/index.md %}) · [Source specification](https://github.com/delzubu/atlas-core/blob/4a656981ba21c6139a3d3750e0cb76f8d706284a/docs/design/components/Forms.md)

The small controls an editing screen is made of (first used by an edition's board-task editor). They sit in the content
region or an Inspector tab, never in the AppBar. Chrome stays quiet: 13px, `surface`, `border`; `accent` marks only focus
and the single primary action.

Source: `packages/app-suite/src/forms/`. Exported from `@atlas/app-suite`.

## Combobox
A type-in dropdown for **one** choice or **several** (chips). Use it instead of a `<select>` whenever the list can be
long, searched on a server, or holds people.

```
┌──────────────────────────────────────────┐
│ (Red ×) (Blue ×)  type to search…        │   input, 30px min, radius-md; focus ring = accent
└──────────────────────────────────────────┘
   ┌────────────────────────────────────┐
   │ ● Green                            │   list: portalled Floating layer, radius-lg, popup shadow
   │   Alice Example   alice@example.com│   active row: hover tint; selected: selected tint
   └────────────────────────────────────┘
```
- **Options** come from a fixed list (`options`, filtered locally) and/or `search(query)` (debounced 150ms, called with `''` on
  open; an answer to an older query never replaces a newer one). Each option has `id`, `label`, optional `detail`
  (right-aligned, muted), `color` (dot) and `disabled`.
- **Single**: the box shows the chosen label; focusing it starts a search. `clearable` adds ×.
- **Multi**: chosen options are chips (× removes); chosen options leave the list; Backspace in an empty box removes the last.
- **Keyboard**: ↓/↑ open and move, Enter picks, Esc closes (and is handled before any dialog behind it), Tab leaves.
- **Accessibility**: `role=combobox` on the input with `aria-expanded`, `aria-controls` and `aria-activedescendant`; the list is
  `role=listbox` (`aria-multiselectable` for multi); focus never leaves the input.
- **Empty and loading** say so in the list ("Nothing found", "Searching…"). The list is never clipped: it is portalled.
- No free text: a value must come from the list. (A picker that accepts typed values is a different control.)

## DateField
A native `<input type=date>` (calendar, keyboard and phones for free) plus quick picks as pills: Today, Tomorrow, Friday,
In a week, End of month, and Clear when set. The pill matching the value is pressed. Values are `YYYY-MM-DD` or `''`.

## Toasts
`ToastProvider` once near the root, `useToasts().push(message, kind, action?)`. A toast is one line at the bottom centre
with an optional text action ("Undo") and ×. Info and success go after 6 seconds; **errors stay** until dismissed. Use for
the outcome of an action ("Saved 2 changes"), never for something the person has to decide.

## FieldConflictDialog
Shown when a save is refused because someone else changed the same fields (HTTP 409). A `SuiteModal` listing each field with
three lines (**You started from**, **Now in Planner**, **You wanted**) and a choice: keep what is there (default) or use mine.
Cancel leaves the draft as it was. Nothing is decided for the person.

## Secret fields
A credential (a password, token or key) that has a value is **always shown masked**, never as an empty box: `••••` and its last 4 characters
(`••••` alone for a short value), in a plain text box so the person can tell which key is in place. This holds wherever the value comes from:
saved here, saved elsewhere, or seeded by the deployment. An empty box means *nothing is set*. A hint under the field says where the value comes
from. Sent back untouched, the mask keeps the value in place; typing replaces it (the box turns into a password box while typing). The value
itself is never sent to a browser. Shared form: `ConnectionFields` (`src/forms/ConnectionFields.tsx`).

## Voice
Verb-first buttons ("Save", "Add label"), sentence case, numerals for counts, one-line empty states.
