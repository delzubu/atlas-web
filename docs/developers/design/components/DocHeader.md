---
title: DocHeader
parent: Component reference
grand_parent: Design
nav_order: 5
nav_exclude: true
---

# DocHeader

[Component reference]({{ site.baseurl }}{% link docs/developers/design/components/index.md %}) · [Source specification](https://github.com/delzubu/atlas-core/blob/4a656981ba21c6139a3d3750e0cb76f8d706284a/docs/design/components/DocHeader.md)

DocHeader is the single 40px line above any document or selected item. It holds the title, the item's state and metadata, the few primary actions, and the doors to everything else (the Inspector toggle and ⋯). It merges what is now a separate toolbar row, a 32px H1 block and a path footer.

## Anatomy (left to right)
1. **Back / forward** (optional, kb and workspace): two icon buttons. The history list is on a long press or in ⋯.
2. **Title** (`title` style, one line, ellipsis): click or press E to edit in place. A `warning-border` dirty dot follows it (aria-label "unsaved").
3. **Metadata chip** (`meta`, `muted`): a one-line summary such as "1648 · Westphalia · 3 tags" (kb) or "12 KB · markdown · 214 lines" (workspace). Clicking it opens a popover with the full fields, the path, "Copy path" and "Open on GitHub". The popover replaces PathFooter and FileInfoChip.
4. **Spacer.**
5. **Primary actions**: at most three, and only one of them uses the `accent` fill (e.g. Save while dirty). Everything else is a quiet `ui` button.
6. **Inspector toggle** (`]`): shows the default tab's name and a count ("Related 5", "Comments 2"). When the Inspector is open it takes the `selected` ground.
7. **⋯ overflow**: every other action of the item, with shortcuts shown. The same list is registered as palette commands.

## Rules
- Height `docheader-h`, ground `surface-2`, bottom `border` hairline, padding-x `space-3`. It sits sticky at the top of the Document region.
- Phone: the metadata chip wraps to a second 24px line; actions collapse into ⋯ except the Inspector toggle.
- Editing mode keeps the same line. Save/Revert replace the primary actions; nothing else moves.
- The map shows the DocHeader only while an item is selected, above the canvas (not over it).

## What the consumer provides
```ts
<DocHeader
  title onRename? dirty?
  meta={{ summary: string, details: Field[], path?: string, externalUrl?: string }}
  nav?={{ back, forward, history }}
  actions={Action[]}            // first 1–3 with primary: true are shown
  overflow={Action[]}           // also registered as suite commands
  inspector?={{ label, count, open, onToggle }} />
```

## Replaces
kb `ExplorerView` toolbar (`:145-168`), the `DetailsPane` header (`:183-236`) and `PathFooter`; workspace `EditorHeader` and `FileInfoChip`.

## Implementation status
`DocHeader`, `DocMenu` and `DocMeta` (the metadata chip and its popover) are in `packages/app-suite/src/document.tsx`. DocHeader adds the Inspector toggle itself.

- `navPosition: 'start' | 'end'`. With 'end', history and previous/next come after the actions, so the title starts the line and shares its left edge with the text.
- `chips` is a full-width second row that wraps (kb: places and tags, shown in full). If the title line itself wraps (a narrow column, e.g. with the Inspector docked), the chips continue on that wrapped line instead of opening a third one, so the header stays two lines tall.
- The ⋯ menu and the metadata popover are portalled and viewport-clamped, so they are never clipped.

| App | Nav | Title / meta | Actions | ⋯ |
| --- | --- | --- | --- | --- |
| kb-explorer | at the end: back, forward, recent; earlier, later | event title; `DocMeta` "1648" → date, location, tags (edit), path; chips row: places and tags | Edit, pending changes, Timeline | Copy link, Refresh, Delete |
| workspace | — | file name + dirty dot; folder and the file-info chip | Visual/Source (Markdown), Changes, Edit/View, Save | Download, History, Revert |
