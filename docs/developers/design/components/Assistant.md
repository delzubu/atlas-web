---
title: Assistant
parent: Component reference
grand_parent: Design
nav_order: 3
nav_exclude: true
---

# Assistant (the agent)

[Component reference]({{ site.baseurl }}{% link docs/developers/design/components/index.md %}) · [Source specification](https://github.com/delzubu/atlas-core/blob/4a656981ba21c6139a3d3750e0cb76f8d706284a/docs/design/components/Assistant.md)

The agent is the suite's one place to ask a question or give an instruction to a hosted ELITEA agent from wherever you are
in Atlas, and to see what it did. It is a small handle on the edge of the page that opens a floating, non-modal window.
The document stays the focus: the agent supplements it and never takes space from it. Every app gets the same agent from
`app-suite` (`src/assistant/`); an app only says where the person is. Preview: [previews/Assistant.html]({{ site.baseurl }}{% link docs/developers/design/previews/Assistant.html %}).
Backend and lifecycle: [assistant-integration.md](https://github.com/delzubu/atlas-core/blob/4a656981ba21c6139a3d3750e0cb76f8d706284a/docs/assistant-integration.md). **Status: implemented.**

## Principles
1. **Quiet until wanted.** The only always-visible part is a 28×48px handle. Nothing else of the agent is on the page.
2. **Where you are is known, not asked.** Every message carries the app, workspace, open document, selection and view in the
   prompt. There is no context UI by default.
3. **Never a surprise.** A running request always says what phase it is in, for how long, and what earlier runs took. The
   provider streams nothing and records no per-tool steps, so progress is phases and a clock, never an invented step list.
4. **You can always stop.** Stop works whatever ELITEA does, and frees you to ask again at once.
5. **Changes are visible.** The apps refresh as the agent writes, and the answer ends with what changed, with Show and,
   where the app can reverse it, Undo.

## Entry points
1. **The handle**: a tab flush to the document's edge (right by default, left in Settings), at a remembered height. Click
   or Enter opens the window. Drag it along the edge, or across to the other edge; ↑/↓ move it when focused.
2. **Ctrl/⌘+.** from anywhere (also closes the window). It works with the handle hidden.
3. **The palette**: "Ask the agent", "New agent thread", "Agent settings", "Hide agent button" / "Show agent button".

Hovering or focusing the handle expands a short pill beside it: **Settings** and **Hide**. Settings opens a modal on click,
never on hover. Hiding keeps the shortcut and palette. Touch: a tap opens the window.

## The handle
`surface`, hairline, `shadow-float`, 16px outline glyph, `z-overlay-panel` (below the AppBar, popups, modals and toasts).
Its corner badge shows the state when the window is closed; the label is in its tooltip and `aria-label`:

| State | Badge | Label |
| --- | --- | --- |
| Idle | none | Ask the agent |
| Working | accent dot (slow pulse) | Agent is working · 2:41 |
| Needs you | `!` on `warning-bg` | Agent needs you |
| Finished, not seen | `✓` | Agent finished |
| Failed, not seen | `!` in `danger` | Agent run failed |

Colour is never the only signal; the pulse stops under `prefers-reduced-motion`. When a run ends while the window is closed,
a toast says so: "Agent finished · 3 changes" with **Undo** where the app offers it, or "The agent run failed".

## The window
Non-modal, like Help's floating mode: default 420×600 beside the handle, moved by the title bar, resized by the corner,
clamped inside the viewport and below the AppBar, size and position remembered per user (`atlas:v1:assistant-ui`). Esc
closes it and returns focus to where it was; there is no focus trap (`role="dialog" aria-modal="false"`). Under 720px it is a
full-screen sheet. Closing it does not stop a run.

```
┌ ⠿  <thread title>                       🕘  ＋  ⚙  × ┐  title bar 32px, surface-2
│                                                       │
│                    You: What is the status …   14:02  │
│  ◔ Working · 2:41                            [Stop]   │  run card
│  ✓ Sent  ✓ Thinking  ● Using Workspace tools  ○ Answer│
│  Your last runs took 3–9 min. You can keep working.   │
│  … answer (Markdown; atlas:// links as chips) …       │
│  3 changes  ~ a.md  ~ b.md        [Undo these changes]│
│ ┌ Ask about a.md, or say what to change…       ┐ [↵] │
└───────────────────────────────────────────────────────┘
```

- **Title bar**: grip, thread title (the first instruction), Threads (history menu), New thread, Settings, Close.
- **Run card** (replaces "Working…"): phase (Sending · Thinking · Using <skill> tools · Waiting for you · Continuing),
  clock from the request, the four-step line (✓ done, ● now, ○ later), and an expectation line: after three finished runs
  "Your last runs took 3–9 min", before that "Agent runs usually take several minutes". Past the slowest earlier run ×1.5
  (at least 10 min, 15 with no history) the card turns `warning` and Stop becomes its filled, main action.
- **Stop** is always enabled and never confirms. It ends polling at once, releases the request on the Atlas side, then asks
  ELITEA to cancel once, best effort. The next request goes to a fresh ELITEA conversation so a runaway run cannot mix with it.
  What arrives later from the stopped run is shown only behind "Show what arrived before it stopped".
- **Needs you**: one checklist of pending tool authorizations (Authorize / Skip; "Change" after a decision) and the agent's
  questions inline (answer field, Approve, Reject).
- **Answer**: Markdown (paragraphs, lists, tables, code, emphasis). `atlas://` links, written or bare, are chips that open
  in the right app; in the installed app they open as tabs.
- **After an answer**: "Finished in 3:05"; a **changes** list (what the agent changed, from the app) with Show and Undo.
- **Not sent / failed / stopped**: a note with the reason and **Edit and retry** (puts the text back in the composer), and
  "Open in ELITEA ↗" for a failed run.
- **Composer**: grows to eight lines; Enter sends, Shift+Enter breaks the line. When empty it offers suggestions for the
  place (a file, a plan, the workspace list); the placeholder names it. While a run is active it explains why it is off.
- **Gates** replace the composer's controls when needed: "Connect the Atlas agent" (one sign-in) and "Add your ELITEA key".
- **Errors** are one sentence with one action (Try again, Connect, Add key). Server wording is a last resort.

## Context
The server follows the person's instruction with an `<atlas_context>` block: one sentence ("Unless the instruction above says otherwise: the current app is
Workspace; no workspace is open; no document is open; nothing is selected; the person is looking at the list of their
workspaces (…); there are no unsaved changes; today is …") and then the exact JSON. What does not apply is stated, so the
agent can tell "nothing is open" from "unknown". Each app reports through `useAssistantContext`: `project`, `document`,
`selection` (`{id, label, kind}`), `view`, and `details` (`kind`, `summary`, `breadcrumb`, `workspaces`). Apps without an
editor integration report the object in the address bar and their breadcrumb.

When a workspace is part of the message, the session adds `details.instructions` (`{path: 'AGENTS.md', revision}`) and the context
sentence names that revision, so the agent re-reads the workspace's `AGENTS.md` only after it changed. Apps do not set it. See
`docs/assistant-integration.md`.

**Context editor** (experiment, off by default; Settings → "Show context editor"): a strip under the title bar shows the
parts that will be sent. A part can be left out of the next message, and ⊕ attaches another object (`RefPicker`).

## Settings (modal)
ELITEA key (save, remove); agent (only when more than one is configured); tools (optional toolkits; required ones are
"Always on"; tools built into the agent are listed read-only); connected tools with Disconnect; the handle (show, side, reset
position); the context editor. Nothing of this is on the main surface.

## Persistence
Window geometry, handle side/height/visibility and the context-editor switch: `atlas:v1:assistant-ui` (per browser). The open
thread: `sessionStorage` per tab (a reload resumes it). Threads and turns: the Assistant service (Cosmos), per user.

## Accessibility
Handle and every icon button have `aria-label` and a tooltip with their shortcut. The handle's hover pill is also shown on focus.
The run card's label is a polite live region; its clock is not announced. Answers are in document order and links are
real links. Reduced motion removes the pulse and transitions. Colour is never the only signal.

## Tokens and parts
`--surface`, `--surface-2`, `--border`, `--hover`, `--accent`, `--muted`, `--danger`, `--success`, `--warning-bg/border`,
`--space-*`, `--radius-*`, `--shadow-float/overlay`, `--z-overlay-panel`, `--handle-w`, `--handle-h`. Reuses `SuiteModal`,
`SuiteMenu`, `Combobox`, `RefPicker`, the Help Markdown renderer, toasts and the palette.
