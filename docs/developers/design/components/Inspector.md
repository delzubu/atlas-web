---
title: Inspector
parent: Component reference
grand_parent: Design
nav_order: 12
nav_exclude: true
---

# Inspector

[Component reference]({{ site.baseurl }}{% link docs/developers/design/components/index.md %}) · [Source specification](https://github.com/delzubu/atlas-core/blob/4a656981ba21c6139a3d3750e0cb76f8d706284a/docs/design/components/Inspector.md)

The Inspector is the one secondary panel on the right of every Atlas app. It is tabbed, docked next to the text on wide screens, and toggled with `]` or the DocHeader toggle. It replaces kb's fixed 20rem Related/Sources/Path column, the workspace's separate History (300px) and Comments (320px) panels, and the map's floating ItemEditor card.

## Anatomy
- **Tab strip** (32px, `surface-2`): text tabs in `ui-strong`, with the count in `caption`. The active tab gets a 2px `accent` underline. A close × sits on the right.
- **Body**: scrolls on its own. Sections inside a tab use `ui-strong` headings and never cards-in-cards.
- **Resize handle** on the left edge: 8px hit area, `accent` at 35% on hover. Width `inspector-w` (360px) by default, clamped to 300–480.

## Behaviour
- **≥1600px (the primary 1920×1080 laptop):** docked and **open by default**. It sits directly beside the 720px text column, and the free margin goes to the left of the text, so relations read alongside the paragraph they describe.
- **1200–1599px:** docked, closed by default. `]` or the DocHeader toggle opens it.
- **<1200px (tablet and phone):** a right overlay drawer with `shadow-overlay` and `z-overlay-panel`. Esc or a click on the scrim closes it.
- Open/closed state, the active tab and the width are remembered **per app** in suite preferences. The user's choice overrides these defaults.
- **Opening a tab from elsewhere:** DocHeader counts, the kb relation summary above the text ("Caused by 2 · After 1"), a comment highlight in the workspace, selecting a map item, and an unconfigured search app each open the Inspector on the right tab.
- It hides in focus mode, just as `data-atlas-aux` panels do today.

## Tabs per app
| App | Tabs (default first) |
| --- | --- |
| kb-explorer | Related · Fields |
| workspace | Comments · History |
| planner | Item (Selection when several) · Links · Assignments · Issues · Plan |

## What the consumer provides
`<Inspector tabs={[{ id, label, count?, render }]} defaultTab />`. The suite owns open state, width and keyboard handling. Apps register tabs through `useInspectorTabs()`, so DocHeader can show the toggle automatically.

## Do / don't
- Do put anything item-scoped and secondary here.
- Don't open two right panels at once. That is the case the workspace allows today.
- Don't float cards over content (the map's ItemEditor). A map selection opens the Item tab.

## Implementation status
`SuiteInspector`, `InspectorTab`, `InspectorToggle` and `useInspector` are in `packages/app-suite/src/document.tsx`. The state lives in `SuiteProvider`; preferences hold `inspectorOpen` (null = the width default), `inspectorTab` and `inspectorWidth`, per app.
