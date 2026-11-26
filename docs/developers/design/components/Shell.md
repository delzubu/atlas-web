---
title: Shell
parent: Component reference
grand_parent: Design
nav_order: 18
nav_exclude: true
---

# Shell

[Component reference]({{ site.baseurl }}{% link docs/developers/design/components/index.md %}) · [Source specification](https://github.com/delzubu/atlas-core/blob/4a656981ba21c6139a3d3750e0cb76f8d706284a/docs/design/components/Shell.md)

Shell is the four-region page frame every Atlas app renders: AppBar on top, then Navigator, Document and Inspector in a row. It is SuiteFrame's layout, re-expressed as a CSS grid so that no region reserves space for floating chrome.

## Grid
```css
.atlas-frame { display:grid; grid-template-rows: var(--appbar-h) 1fr;
  grid-template-columns: var(--nav-w) minmax(0,1fr) var(--insp-w); }
.atlas-document-body { display:grid; grid-template-columns: 1fr minmax(0, var(--measure)) var(--space-5); }
```
- `--nav-w` is `panel-w` (pinned), `rail-w` (collapsed) or `0` (overlay).
- `--insp-w` is `inspector-w` when docked and open, `0` otherwise.
- The document body left-aligns the `measure` column with the DocHeader title. Surplus width becomes right margin.

## Breakpoints (design target: 1920×1080 laptop, viewport ≈1920×950)
| Viewport width | Navigator | Inspector | Text |
| --- | --- | --- | --- |
| ≥1600px (primary) | pinned 320 | docked, open by default, 360 | 720 column, adjacent to Inspector |
| 1200–1599px | pinned | docked, closed by default | 720 column, centered |
| <1200px (tablet, phone: secondary) | overlay (☰) | overlay | full width, 16px gutters |

Vertical budget: `appbar-h` + `docheader-h` = 76px of chrome. Nothing else sits above the text, and nothing floats over it.

## What the consumer provides
Content for the regions only: `<SuiteFrame appBar={{breadcrumb}} navigator={<AtlasTree…/>} inspector={{tabs}}>{document}</SuiteFrame>`. The suite owns widths, persistence, focus mode, keyboard handling (`[`, `]`, `/`, ⌘K) and breakpoints.

## Navigator tools and the rail
`SuitePanel tools={PanelTool[]}` puts the app's frequent commands in an icon strip under the navigator heading: create first, then the selected item's commands.

| App | Create | Selected item |
| --- | --- | --- |
| workspace | New file, New folder, Upload, Refresh | Rename, Download, Delete |

A canvas editor may be the exception for the selected item: it can put the selection's commands on a floating bar over the selection (with transform handles), so the strip holds only the create tools. The strip then never grows with the selection, and nothing wraps. Edition apps' rows and notes are in their projects' `docs/design/components.md`.

On wide screens a closed navigator leaves a `rail-w` strip: Show panel, Search, then the same tools. The navigator is never gone without a trace, and the tools stay one click away (an app may close its navigator by default). Narrow screens use the overlay instead; focus mode hides it.
