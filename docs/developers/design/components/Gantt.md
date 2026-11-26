---
title: Gantt
parent: Component reference
grand_parent: Design
nav_order: 9
nav_exclude: true
---

# Gantt chart (Planner)

[Component reference]({{ site.baseurl }}{% link docs/developers/design/components/index.md %}) · [Source specification](https://github.com/delzubu/atlas-core/blob/4a656981ba21c6139a3d3750e0cb76f8d706284a/docs/design/components/Gantt.md)

The Document of Planner's plan view: items as bars on a time axis, the same rows as the navigator. It is a content
component, not shell chrome: the shell around it (AppBar, Navigator, DocHeader, Inspector) is unchanged and no new
region exists.

## Anatomy
- **Axis** (44px, `surface-2`): two tiers. The minor unit is the smallest of hour, day, week, month, quarter, year, decade
  and century that is at least 34px wide; the major unit is the next one up. Labels come from the plan's calendar, real
  or fictional (month names, eras). It stays on top while the rows scroll.
- **Rows** (24px, `row-compact`): one per navigator row, in the same order, with the same expand state and grouping.
  In a **swimlane view** (the plan's default view, chosen in Plan settings; the person's own choice per plan wins) the
  chart is cut into **bands**, one per lane, with "No lane" on top and the defined lanes below in their order. A band
  spans all the rows of its lane (at least two, so an empty lane can be aimed at), carries a tint of the lane's colour
  across the whole chart and a 28px **gutter** on the left holding the lane title rotated 90 degrees. The navigator
  keeps a header row per lane (selectable, with the count of tasks). Lanes are never hidden by a filter. An ancestor of a
  lane's items appears under each lane it has items in. In an editable plan an empty band says "Drag a task here".
- **Backdrop**: a faint grid at the minor unit, `surface-2` shading on non-working days (when a day is at least 5px wide),
  a dashed line at the status date, an `accent` line at today.
- **Marks**:

| Item | Mark |
| --- | --- |
| Task | Rounded bar, 12% tint with an outline; solid fill for the completed share |
| Summary | A bracket spanning its children |
| Milestone | A diamond |
| Hammock | A dashed bracket between its anchors |
| Critical | A 2.5px `danger` outline (and the legend entry "Critical") |
| Manual | A dashed outline |
| Locked | A dotted outline |
| Uncertain dates | A faint band from the earliest start to the latest finish behind the bar |
| Baseline | A 3px muted bar under the item, for the baseline chosen in the Plan tab |
| Deadline | A `danger` triangle on the deadline |
| Issue | ⚠ before the label |
| Link | An elbow arrow; `accent` when either end is selected |

Colour is never the only signal: critical is also heavier, manual dashed, issues carry a glyph, and lane colours
(`hl-1…6`) always come with the lane's name in its header.

## Behaviour
- **View mode** (`E` toggles it off; editing is the default): click selects; drag on empty space or on a bar pans along time; wheel scrolls rows;
  Ctrl/Cmd+wheel zooms about the pointer; Shift+wheel (or a sideways wheel) pans.
- **Edit mode** (on by default; `E` or the "Editing / Read-only" toggle switches it; any edit gesture or command turns it
  on, so a control is never silently disabled by the mode): dragging a bar moves it. A manual task gets new dates; an automatic
  one gets a *start no earlier than* constraint (a ghost shows the new dates while dragging). Dragging a bar's
  left or right edge resizes it (the duration, or a manual date). Dragging from the round handle at a bar's start or
  finish onto another bar's start or finish draws a link, and the two ends chosen make its type: finish→start FS,
  start→start SS, finish→finish FF, start→finish SF. A link that would close a loop is refused with a message. In a
  swimlane view, dragging a bar up or down onto another band highlights the band and changes the task's lane (its dates
  stay; a locked task stays put); dragging a summary moves its tasks. Esc cancels any drag. Every gesture is one batch
  and one undo step.
- **Selection** is shared with the navigator. Selecting an item anywhere (navigator, a link in the details sidebar, an issue,
  the address bar's `?item=`) **reveals** it on both axes: the rows scroll to its row, and the chart pans to its dates,
  zooming out when the bar is wider than the window. It does not move when the item is already comfortably in view.
  `.` reveals the selection again. Selecting a bar selects and scrolls the navigator row.
- **Hover** shows the bar's dates, whether it is critical, and (on bars) the link handles.
- **Keyboard**: Z fits the plan, T goes to today, +/− zoom, ←/→ pan while the chart has focus, Insert adds a task after
  the selection, Delete deletes, L links the selection, Alt+Shift+→/← indent/outdent.

## Resource usage
With the navigator's Resources tab open, the Document shows the usage view on the same axis: one lane per resource
(groups first), a bar per day for its load, a dashed line for its capacity, over-allocated days in `danger` with diagonal
hatching, and a ⚠ in the lane header. The items assigned are links below the lane.

## Navigator rows
A type icon (task, summary, milestone, hammock, subproject, tinted by type), the name, and **one** detail at the right in
`meta`, chosen with the "Show" chip (Finish, Start, Duration, Assignee, FTE, % complete, Slack, None). A ⚠ appears when
the item has an issue. The "View" selector in the DocHeader (Outline, or Swimlanes: <view>) splits the outline into lanes; lane rows are
drop targets (dropping an item on one changes its lane). Dragging rows reorders or reparents (drop on the top or bottom quarter of a row to place
before or after it, on the middle to make the item its child).

## Do / don't
- Do keep the navigator and the chart on the same rows; do not add columns to the navigator, add a value to "Show".
- Don't float toolbars over the chart: commands live in the navigator tools, the DocHeader and right-click menus.
- Don't use colour alone for status.

## Implementation status
`apps/planner/web/src/Gantt.tsx` (chart), `Axis.tsx`, `Usage.tsx`, `view.ts` (camera, reveal), `geometry.ts` (hit testing),
`edits.ts` (gestures as commands). Tested in `logic.test.ts`, `time.test.ts` and the browser walk-through in
`apps/planner/web/scripts/e2e.mjs`.
