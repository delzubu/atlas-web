---
title: AppBar
parent: Component reference
grand_parent: Design
nav_order: 2
nav_exclude: true
---

# AppBar

[Component reference]({{ site.baseurl }}{% link docs/developers/design/components/index.md %}) · [Source specification](https://github.com/delzubu/atlas-core/blob/4a656981ba21c6139a3d3750e0cb76f8d706284a/docs/design/components/AppBar.md)

AppBar is the single 40px ink line of global chrome at the top of every Atlas app. It replaces the floating "Atlas / App" anchor, the panel-launcher button and the 64–108px content insets they forced.

## Anatomy (left to right)
1. **Rail toggle** (`[`): collapses or expands the Navigator. On phone it becomes ☰ and opens the Navigator overlay.
2. **Atlas mark + app name**: opens the existing app-switcher popup (`atlas-suite-menu` contents unchanged).
3. **Breadcrumb**: the path of the current item, in `ui` and `muted`, with the last segment in `fg`. It truncates from the left, and each segment is a link. The app supplies it.
4. **Search field** (flex, max 360px): a button styled as an input ("Search or run a command…  ⌘K"). It opens the existing command palette.
5. **Trailing slot**: the work status (a dot plus a word: "2 unsaved changes", "Saving…", "Search failed"; shown only when there is something to say) the help button (`?`, F1; shown by apps that have help pages, see [Help]({{ site.baseurl }}{% link docs/developers/design/components/Help.md %})) and the account button (initials; menu: who, Atlas home, Sign out). There is no theme toggle, because Atlas has one light theme.

## Rules
- Height `appbar-h` (40px), ground `chrome-ink`, text `on-chrome` (secondary `on-chrome-muted`), a 24px mark and a 15px semibold app name. It is the page's frame: it has weight so the white page below can stay quiet.
- There are no app-specific buttons in the AppBar. App actions belong in DocHeader, ⋯ or the palette.
- In focus mode the bar stays (it is only 36px); the navigator toggle becomes an "Exit Focus" button.
- On phone the breadcrumb shows only its last crumb and the status shows only its dot.
- Use `z-chrome`. It is part of the grid, never `position: fixed` over content.

## API (`@atlas/app-suite`)
SuiteFrame renders the AppBar; apps feed it through hooks while a component is mounted:
- `useSuiteBreadcrumb([{ label, onSelect? }, …])`: the last crumb is the current page; earlier crumbs with `onSelect` are links.
- `useSuiteStatus({ state: 'ok' | 'busy' | 'dirty' | 'error', label, actions? } | null)`: when several components report, the most urgent wins (error > dirty > busy).
- **`actions` (a `MenuItem[]`) make the status a menu button.** Work that spans items belongs here, not in any one item's DocHeader. For example, kb's "2 unsaved changes ▾" opens Review & commit… and Discard all…; see [kb-status-menu.png]({{ site.baseurl }}{% link docs/developers/design/screenshots/kb-status-menu.png %}).
- **Reviewing:** the review screen keeps the verdict in its own DocHeader: the commit message, **Commit** (primary), Discard all and Close. The list below holds only the per-file changes; see [kb-review-header.png]({{ site.baseurl }}{% link docs/developers/design/screenshots/kb-review-header.png %}).
- The account comes from `AuthProvider` automatically when a `SuiteProvider` is above it. Sign out is the launcher's sign-out (cached server config dropped, Atlas session ended), behind the app's unsaved-work guard.

| App | Breadcrumb | Status |
| --- | --- | --- |
| kb-explorer | repo / view / event (repo focuses the search) | N unsaved changes |
| workspace | workspace (opens the picker) / path segments | N unsaved files, Saving… |
| planner | workspace / plan / item | Scheduling…, Working on a long operation…, N issues ▾ (Show issues), Action failed |

## Replaces
`.atlas-anchor`, `.atlas-brand`, `.atlas-launcher`, `.atlas-content-inset` / `.atlas-workspace-content` padding-top, and `.atlas-panel` padding-top (packages/app-suite/src/style.css).
