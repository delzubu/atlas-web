---
title: Editor
parent: Component reference
grand_parent: Design
nav_order: 6
nav_exclude: true
---

# RichTextEditor

[Component reference]({{ site.baseurl }}{% link docs/developers/design/components/index.md %}) · [Source specification](https://github.com/delzubu/atlas-core/blob/4a656981ba21c6139a3d3750e0cb76f8d706284a/docs/design/components/Editor.md)

A WYSIWYG editor for prose that is stored as **markdown**: markdown goes in, markdown comes out. First used for the
description and comments of a board task in an edition app. For files and documents that are edited as a whole, Workspace has its own
editor; this one is the small, embeddable kind.

Source: `packages/app-suite/src/editor/` (Milkdown on ProseMirror). It has its own entry, `@atlas/app-suite/editor`, so apps that do
not edit rich text do not bundle it.

```
┌─ B  I  •  1.  <>  🔗 ───────────────────  93 / 4000 ┐   toolbar: surface-2, 26px buttons; counter shows near the limit
│ Writing area. **Bold**, lists, links, code render as  │   15px body type, 1.5 line height
│ you type.                                             │
└───────────────────────────────────────────────────────┘
```

## Behaviour
- **Markdown in, markdown out.** `value` is the starting text (not reactive: give the editor a new `key` to load another text).
  `onChange(markdown, edited)` reports every change.
- **`edited` is the truth about a change.** The editor normalises markdown it loads (bullet characters, escapes), so the text it
  reports may differ from the stored text although nobody typed. `edited` compares with what the editor first showed. A form that
  saves "what changed" must use `edited`, never compare the text with the stored text: an untouched field must not look changed.
- **Toolbar**: bold, italic, bulleted and numbered list, inline code, link (an inline address field, Enter applies). Shortcuts
  Ctrl/⌘+B and +I work in the area. Buttons never take focus from the text.
- **Ctrl/⌘+Enter** calls `onSubmit` (comment boxes).
- **`maxLength`** only shows a counter (from 80% of the limit; red over it). The owner decides what going over means.
- **`disabled`** shows the text on a tinted ground and hides editing.
- Keyboard users reach the toolbar by Shift+Tab from the area; the area has an accessible name (`label`).

## Not here
Tables, images, headings in the toolbar and comments anchored to text (those belong to Workspace's document editor).
