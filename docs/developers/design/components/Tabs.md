---
title: Tabs
parent: Component reference
grand_parent: Design
nav_order: 19
nav_exclude: true
---

# Tabs (installed app only)

[Component reference]({{ site.baseurl }}{% link docs/developers/design/components/index.md %}) · [Source specification](https://github.com/delzubu/atlas-core/blob/4a656981ba21c6139a3d3750e0cb76f8d706284a/docs/design/components/Tabs.md)

In the installed app (the Atlas PWA window), the start page becomes a **tab host**. The start screen is the first tab, and each tool you open runs in a tab of its own. Several tabs can hold the same tool. In a browser tab nothing changes: the browser's own tabs do this job, and Ctrl/Shift/middle-click already open links there.

![Two kb tabs in the installed app]({{ site.baseurl }}{% link docs/developers/design/screenshots/tabs-1920.png %})

## Anatomy
```
┌ [A Atlas] [◷ Eastholm founds a colony ×] [◷ Westmarch signs a treaty ●×] [+] ───────── tab strip, 34px, --surface-2
├ ▣ KB Explorer ▾ / kb / Timeline / Eastholm founds a colony        ⌘K ─── the tool's own AppBar (ink), unchanged
│ navigator │ document                                │ inspector
```
- The **tab strip** sits above each tool's own AppBar. It is window chrome, not a second app bar, so it stays slim and quiet: `--surface-2` ground, muted inactive tabs.
- The **active tab** takes the colour of what is under it. A tool tab is ink, joined to the tool's ink AppBar. The start tab is white, joined to the white start page.
- **A tab** shows the tool's glyph, the tool's current title (the last breadcrumb), a dot while the tool holds unsaved changes, and ×. Middle-click closes it.
- **+** returns to the start tab, where you pick a tool.
- The vertical cost is 34px on top of the tool's 40px AppBar.

## Behaviour
- **What opens a tab (browser rules).**
  - A tool card on the start tab.
  - Inside a tool: middle-click or Ctrl+click on an item, "Open in new tab" in its right-click menu, and Ctrl/Shift/middle-click or `target=_blank` on a same-origin link.
  - A plain click navigates within the current tab.
  - Tabs opened from inside a tool open in the background, next to that tool's tab.
- **Hidden tabs stay alive.** They are hidden with `visibility`, not unmounted, so a map keeps its view and an editor keeps its buffer.
- **Restart** reopens the same tabs at the URL each tool was showing. In-memory state is not kept: unsaved edits and the kb back/forward trail are lost.
- **Closing** a tab with unsaved changes asks first.
- **History is per tab.** kb's back/forward (and any tool history) moves within its own tab. It never steps another tab or the window.

## Keys
The browser keeps Ctrl+T, Ctrl+W and Ctrl+Tab for its windows, the installed app's window included, so the tab strip uses keys a page actually receives. They work with focus anywhere, inside a tool too.

| Key | Action |
| --- | --- |
| Ctrl+PageDown / Ctrl+PageUp | Next / previous tab |
| Ctrl+1 | Start tab |
| Ctrl+2 … Ctrl+8 | Tab 1 … 7 |
| Ctrl+9 | Last tab |
| Alt+W | Close the tab |
| Alt+Home | Start tab |

## Sign-in
- **One sign-in per tool.** Each tool has its own sign-in client. All tabs in a window share that window's session (sessionStorage), so each tool signs in once per window.
- **Sign-in never runs inside a tab.** The identity cookie is SameSite=Lax, so a tool without a session asks the host instead. The host loads that tool at top level with `?atlas-return=1`; the tool signs in (silently, if the Atlas session is live) and returns to the tabs.
- **This reloads every tab.** When another tab holds unsaved changes, the host does not do it straight away. It shows "Sign in and reload tabs" in the waiting tab instead.
- **Token refreshes are serialised.** Refreshes run one at a time across tabs through a Web Lock, because the identity server revokes a refresh token presented twice.
- **Sign-out** from any tool leaves the whole window.

## Implementation
| Part | Where |
| --- | --- |
| Host: strip, frames, persistence (`atlas:tabs`), keys, sign-in hand-off | `packages/app-suite/src/tabs/TabHost.tsx`, `tabs.css` |
| Protocol: frame detection (`atlas-tab:<id>` frame names), messages, `openInNewTab`, `tabAction` | `packages/app-suite/src/tabs/embed.ts` |
| Tool side: title, dirty and URL reports, key forwarding, link interception | `SuiteProvider` in `packages/app-suite/src/suite.tsx` |
| Sign-in hand-off and return | `requireLauncherAuth` in `packages/app-suite/src/auth/gate.ts` |
| Refresh lock | `OAuthClient.refresh` in `packages/app-suite/src/auth/oauthClient.ts` |
| kb per-tab history | `useNavigation` in `apps/kb/web/src/useSelectedEvent.ts` |

- **When the host is active:** `display-mode: standalone` (or `window-controls-overlay`) at top level. For development, `?atlas-host=1` turns it on in a browser tab and `?atlas-host=0` turns it off.
- **Not used: the manifest's `display_override: ["tabbed"]`.** It is Chromium's experimental native tab strip, and would stack a second strip on top of this one where supported.

## Limits
- **Memory:** each tab is a whole tool. Map tabs are the heaviest.
- **Map items have no URL**, so a map tab opens at the map, not at an item.
- **Tabs of one tool share its local preferences** (panel width, pins). The last change wins.
