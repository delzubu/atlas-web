---
title: AtlasTree
parent: Component reference
grand_parent: Design
nav_order: 4
nav_exclude: true
---

# AtlasTree

[Component reference]({{ site.baseurl }}{% link docs/developers/design/components/index.md %}) · [Source specification](https://github.com/delzubu/atlas-core/blob/4a656981ba21c6139a3d3750e0cb76f8d706284a/docs/design/components/AtlasTree.md)

AtlasTree is the one navigator for every Atlas app: a keyboard-first, filterable tree of folders and items, driven by a per-app data adapter. It replaces kb's NavPanel, the workspace's FileTree and the map's Navigator. Those three have separate row, hover, selection and twisty styles today.

## Anatomy
- **Header row** (inside SuitePanel): the filter field (`/` focuses it, Esc clears it), a filters button with a count badge that opens the filter page (kb), and a ⋯ menu for panel actions such as new file, add layer and refresh.
- **Chips row** (only when filters are active): removable chips plus the "N of M" count in `caption`.
- **Rows**, from left to right:
  1. indent (12px per level)
  2. chevron twisty
  3. optional checkbox (map visibility, including indeterminate)
  4. type icon or color dot
  5. label (`ui`)
  6. trailing `meta` text (kb dates, file state) and badges (dirty dot, comment count)
  7. hover actions (at most two, e.g. ＋ sublayer)
- **Sticky group headings** (`ui-strong`, uppercase optional). kb uses them for centuries.

## Behaviour
- Row height `row-compact` / `row-default` by density; `row-touch` on coarse pointers. Labels ellipsize; there is no wrapping. The current kb rows wrap to two lines, which halves density.
- States:
  - hover `hover`
  - selected `selected` with `fg` text, trailing text included
  - focus ring `accent`
  - drop target: a 2px `accent` line (before/after) or `selected` fill (inside)
- Keyboard: ↑↓ move, → expands or enters, ← collapses or goes to the parent, Enter opens, type-ahead matches labels, F2 renames, Del deletes (with confirm), and ⇧/⌘ extend the selection when `multiSelect` is set.
- Remembers expansion and scroll per app (reuses `readStored` / `useRememberedScroll`).
- Collapsed to rail (`rail-w`): shows only the app's top-level group icons. Clicking one expands the panel as an overlay.

## What the consumer provides
Rows, already flattened (what is visible, at which depth), plus handlers. `AtlasTree` lives in `packages/app-suite/src/tree.tsx`:
```ts
<AtlasTree
  label="Workspace files"
  rows={[{ id, depth, text, label?, icon?, trail?, actions?, expandable?, expanded?, selected?, current?, heading?, className?, parentId? }]}
  cursor? onCursor?(id, { shift })          // keyboard moves; controlled or internal
  onToggle?(id, expand) onActivate?(id)      // Enter / plain click on a leaf
  onRowClick?(id, event)                     // replaces the default click (multi-select)
  onKeyDown?(event, cursor)                  // app keys first (copy/paste, F2, Delete); preventDefault to claim
  rowProps?(row)                             // drag-and-drop, context menu, data-*, aria-label
  render?(row) afterRow?(row)                // custom row content; drop previews after a row
  contextMenu?(row) => MenuItem[]           // right-click, Shift+F10, the menu key
  ancestors?(id) empty? listRef? listProps? />
```
- `treeMove(rows, cursor, key)` is the keyboard model, exported for tests and custom lists.
- `TreeFilter` is the filter field above the tree.
- `TreeIcon kind` gives the type-tinted icons; `fileIconKind(name)` picks a kind by extension.
- Right-click menus render through `SuiteContextMenu` (`packages/app-suite/src/menu.tsx`).

| App | Rows | Kept from the app |
| --- | --- | --- |
| kb-explorer | era headings, folders, events (date + title via `render`) | filters page, full-text search, select-on-move; row menu: Open, Copy link, Delete event |
| workspace | folders and files (type icons, dirty dot, ⋯ menu as `actions`); row menu: cut, copy, paste, rename, download, delete | multi-select, clipboard keys, F2, Delete, native drag-and-drop, context menu, lazy folders |

## Do / don't
- Do keep app-specific meaning in the adapter: icons, trailing text, badges.
- Don't add a second toolbar above the tree. Panel actions go in the header ⋯ or the palette.
- Don't use emoji icons. `TreeIcon` has folder, folder-open, file, file-md, file-image, file-data, file-code, layer, Planner's task, summary, milestone, hammock and subproject, and Workspace's bundle (a page with pages behind it, a folder shown as one item), tinted by type.
