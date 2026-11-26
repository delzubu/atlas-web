---
title: "Touch and small screens"
parent: Design
grand_parent: Developers
nav_order: 4
---

# Touch and small screens

This guide is the human-readable website edition of the [source specification](https://github.com/delzubu/atlas-core/blob/4a656981ba21c6139a3d3750e0cb76f8d706284a/docs/design/mobile.md). It preserves the design rules and their identifiers; implementation status and measurements are those recorded in the source, not a fresh audit.

The mobile layer of the Atlas design system. [ui_design.md]({{ site.baseurl }}{% link docs/developers/design/ui_design.md %}) designs for a laptop and says tablet and phone "must work but
are not optimised". This file says what "work" means. It also says how an app that chooses to put **phone or tablet first** does it,
so every such app feels the same.

**Status: foundation, 1 Nov 2026.** Written for Languages (epic #106, the first app that puts the phone first) and kept generic
for the apps that follow. The shell does part of it today (see *What the shell does today*). The rest is the rule for new work. The
gaps are listed at the end.

## Start with the job, not the device

A laptop is usually the main creation surface. A tablet is well suited to reading and minor updates; a phone is useful for short review, confirmation and focused tasks. The target levels below distinguish an app that must remain usable on small screens from one whose principal workflow is deliberately designed for touch.

The guide retains the original implementation status and open gaps so readers can distinguish requirements from completed features.

## Relation to the rest of the system

- **Same system, a different shape.** Tokens, colours and their jobs, type styles, icons, voice, the four regions and the "one home
  per command" rule all stay as [ui_design.md]({{ site.baseurl }}{% link docs/developers/design/ui_design.md %}) defines them. On a small screen the regions are *arranged* differently,
  and two pieces are added: the [Action bar]({{ site.baseurl }}{% link docs/developers/design/components/ActionBar.md %}) and the [Sheet]({{ site.baseurl }}{% link docs/developers/design/components/Sheet.md %}). Nothing is invented per app.
- **Brand.** [product-brandbook.md]({{ site.baseurl }}{% link docs/developers/design/product-brandbook.md %}) §12.4 applies unchanged. A phone screen is a working surface, and identity
  colours stay on the mark only. A small screen is no reason to add colour.
- **Conflicts.** `ui_design.md` wins for desktop. This file wins below 1200 px and on touch.

## Targets: which apps optimise for what

Which device is for which job (create on a laptop, read and make minor updates on a tablet, review and confirm on a phone) is in [ux.md]({{ site.baseurl }}{% link docs/developers/design/ux.md %}), *Devices follow the job*. This section says how an app declares and meets its level.

Every app declares one level in its `AGENTS.md`. Once the catalog has the field (`web.targets`, see *Adopting it*), it declares
it there too.

| Level | Meaning | Who |
|---|---|---|
| `laptop` (default) | Laptop first. Tablet and phone must not break: no lost commands, no horizontal page scroll, a readable text column. They are not optimised. | Every app today |
| `touch` | Fully usable on a tablet and a phone for its main tasks. Every rule below applies, and the app may keep laptop-only screens (editors, big canvases) if it says so on that screen. | Study, Workspace viewer (candidates) |
| `phone-first` | The phone is a primary target next to the laptop. The main task is designed on a 390 px portrait screen first, then widened. Every rule below applies to every screen. | Languages |

## Form factors

Layout follows the **width**. Touch sizing follows the **pointer**, whatever the width (touch laptops and tablets with
keyboards exist). Never infer one from the other.

| Class | Width | Typical | Layout |
|---|---|---|---|
| Phone | < 768 px | 360–430 portrait; up to ~930 landscape at low height | One column. Navigator and Inspector are sheets. Action bar at the bottom. |
| Tablet | 768–1199 px | 768–1024 portrait, 1024–1180 landscape | One column of content, up to `measure`. Navigator is an overlay; Inspector is a sheet from the right (or the bottom in portrait). |
| Laptop | ≥ 1200 px | as in `ui_design.md` | As in `ui_design.md`. |

- **The query rules:**
  - `@media (max-width: 767px)` means phone. This is the threshold the shell already uses.
  - `@media (max-width: 1199px)` means smaller than a laptop.
  - `@media (pointer: coarse)` means touch sizing.
  - `@media (hover: none)` means nothing may depend on hover.
- **Height matters on a phone.** A landscape phone is about 360 px tall, and with the on-screen keyboard open a portrait phone has
  300–400 px. Design the main task to work at **360×640 with the keyboard closed** and **390×420 visible with it open**.
- Use `dvh` (or the `visualViewport` height) for full-height layouts, never `100vh`: on phones `vh` includes the browser bars.

## Principles

1. **One task per screen.** A phone screen does one thing: read a list, answer a question, read a result. Secondary content is a
   tap away (a Sheet, the ⋯ menu), never squeezed beside the task.
2. **The primary action is in reach of the thumb.** On a phone, a screen's one primary action sits in the
   [Action bar]({{ site.baseurl }}{% link docs/developers/design/components/ActionBar.md %}) at the bottom, not in the DocHeader. The DocHeader keeps the title and ⋯.
3. **Content stays loud; chrome shrinks.** Chrome stays one line tall (AppBar and DocHeader at 40 px each). On a phone the
   breadcrumb shows only its last crumb, and the DocHeader's metadata chip moves into ⋯ or wraps to a second 24 px line
   (`DocHeader.md`). The vertical budget on a phone is AppBar + DocHeader + Action bar + safe areas: about 136 px plus the insets.
4. **Nothing depends on hover or on a precise pointer.** No hover-only information, no tooltip-only labels, no right-click-only
   commands (long-press opens the same menu, and the command also lives in ⋯ or the palette), no drag-only interaction (every drag has a tap
   alternative, as the learning `DropBoard` has: tap an item, then tap where it goes).
5. **No horizontal page scroll, ever.** `document.documentElement.scrollWidth <= window.innerWidth` on every screen at 360 px. Only
   an element whose content is genuinely wide (a table that cannot stack, code, a chart) scrolls on its own, inside its own box,
   with a visible edge.
6. **The keyboard is part of the layout.** When the on-screen keyboard is open, the focused field and the action that submits it
   stay visible (see *Typing*).
7. **The system gestures belong to the system.** No custom edge swipes (they fight the OS back gesture), no pull-to-refresh of
   our own, no swipe-to-delete. Back means the browser's back: each screen of a phone-first flow has a URL or history entry,
   so the system back gesture goes where the person expects.
8. **Same words, same places.** A command has the same label on every form factor. Only its home moves (DocHeader action →
   Action bar, Inspector tab → Sheet). Help pages describe both.

## Regions on a phone and a tablet

![Knowledge base on a phone]({{ site.baseurl }}{% link docs/developers/design/screenshots/kb-phone.png %})

![Character sheet on a tablet]({{ site.baseurl }}{% link docs/developers/design/screenshots/npc-sheet-tablet.png %})

These source screenshots illustrate different form factors, not a claim that every app has completed its mobile adaptation.

| Region | Laptop | Tablet | Phone |
|---|---|---|---|
| AppBar | 40 px ink frame | the same; ☰ opens the navigator | the same; ☰; breadcrumb shows its last crumb only; the app name is hidden |
| Navigator | pinned 320 px or rail | overlay from the left (`shadow-overlay`) | [Sheet]({{ site.baseurl }}{% link docs/developers/design/components/Sheet.md %}) from the left, full height, max 85 % width. A phone-first app may hide it on focus screens (below). |
| DocHeader | title, meta, ≤ 3 actions, Inspector toggle, ⋯ | title, ≤ 2 actions, ⋯ | title and ⋯; at most one icon action; the primary action moves to the Action bar |
| Document | `measure` column | `measure` column, centred, `space-4` padding | full width, `space-4` padding (16 px), plus the safe-area insets |
| Inspector | docked 360 px | sheet from the right (landscape) or bottom (portrait) | bottom [Sheet]({{ site.baseurl }}{% link docs/developers/design/components/Sheet.md %}), opened by the DocHeader toggle |
| Action bar | none (actions stay in the DocHeader) | optional, for focus screens | the primary action of the screen; see [ActionBar]({{ site.baseurl }}{% link docs/developers/design/components/ActionBar.md %}) |
| Modals (`SuiteModal`) | centred dialog | centred dialog | full-screen Sheet |
| Help | floating or docked window | sheet from the right (exists) | full-screen sheet (exists, `Help.md`) |
| Agent | edge handle + window | the same | full-screen sheet (exists, `Assistant.md`) |

**Focus screens.** A phone-first app may mark a screen as a focus screen (answering questions, a quick capture). It hides the
navigator's ☰ in favour of a close or back control in the DocHeader, and uses the [FocusTask]({{ site.baseurl }}{% link docs/developers/design/components/FocusTask.md %}) layout.
On a laptop the same screen keeps the normal shell.

## Touch targets and spacing

- **Minimum target 44×44 px** (`row-touch`) under `(pointer: coarse)`. Keep at least `space-2` (8 px) between two targets, or make them one
  joined control (a segmented control).
- **Choice answers, list rows and menu items** are full-width rows of at least `row-touch`. The whole row is the target, not
  the radio dot.
- **Icon-only buttons** keep their 16 px icon inside a 44 px target and their `aria-label`. On touch the tooltip is not
  available, so an icon button whose meaning is not obvious gets a visible label on a phone.
- **Density.** Compact rows (`row-compact`) are never used under `(pointer: coarse)`.

## Type on small screens

- The styles are unchanged (`ui`, `meta`, `body`, `heading`, `title`). Nothing is smaller than `meta` (12 px) on a phone; `caption`
  (11 px) is for counts only.
- **Inputs are at least 16 px** (`text-input-touch`) under `(pointer: coarse)`. iOS zooms the page on focus into a smaller input, which
  breaks the layout.
- **The thing being worked on may be larger.** On a focus screen the prompt uses `heading` (18 px), and the answer options and
  typed answers use `body` (15 px) for options and `text-input-touch` for fields.
- Lines wrap. Never truncate a prompt, an answer or a correction with an ellipsis. Truncation is for titles in chrome only.

## Typing

On every text input a person answers or writes in:

| Attribute | Value | Why |
|---|---|---|
| `lang` | the language of the expected text (`de`, `en`, …) | the OS keyboard picks the right language and suggestions |
| `autocapitalize` | `off` for single words and answers to be checked, `sentences` for free text | a capital letter can be the mistake being tested |
| `autocorrect` | `off` for answers to be checked | the OS must not "fix" the answer |
| `spellcheck` | `false` for answers to be checked | no red squiggles hinting at the right spelling |
| `autocomplete` | `off` | no history of earlier answers |
| `inputmode` | `text` (or `numeric`, `email` as fitting) | the right keyboard |
| `enterkeyhint` | the action Enter will take: `done`, `next`, `send`, `go` | the keyboard's Enter key says it |

- **Enter** does the screen's primary action (for example, Check, then Next). Shift+Enter makes a new line in multi-line fields.
- **Keep it in view.** When an input is focused, scroll it into view above the keyboard (`visualViewport` resize →
  `scrollIntoView({block: 'nearest'})`). The Action bar sits on top of the keyboard: its bottom is `visualViewport.height`, not
  the layout viewport.
- **Autofocus** only where typing is the screen's single task (a typed answer). It opens the keyboard. Elsewhere, never
  autofocus on a phone.
- **Special characters.** When the expected text needs characters the person's keyboard may lack, show a **character bar**
  directly under the field: one 44 px key per character, which inserts at the caret and keeps focus in the field. The characters come from the content
  (for example, a language pair's `special_characters`), never from a guess. It is hidden while the field is not focused.

## Components added by this layer

- [ActionBar]({{ site.baseurl }}{% link docs/developers/design/components/ActionBar.md %}): the bottom bar holding a screen's primary action on a phone.
- [Sheet]({{ site.baseurl }}{% link docs/developers/design/components/Sheet.md %}): a panel that slides over the page from the bottom or a side; it is the phone form of the Inspector,
  modals and the navigator.
- [FocusTask]({{ site.baseurl }}{% link docs/developers/design/components/FocusTask.md %}): the layout for one-question-at-a-time work (practice, tests, quick capture).

**Patterns** (no component of their own):

- **Tables become cards on a phone.** Below 768 px a data table becomes a list. Each row is a card: the first column is the
  card's title (and its link), the rest are `label: value` lines in `meta`. Sorting stays in a Sort menu; filters become chips above
  the list. A table may stay a table only if it has at most three narrow columns.
- **Segmented control** (2–4 options, mutually exclusive, e.g. a filter): one joined control, each segment ≥ 44 px tall,
  `role="radiogroup"`. Wider than the screen means it should be a select instead.
- **Stepper** (a small number, e.g. a daily goal): − value + with 44 px buttons; typing the number is allowed.
- **Toasts** sit above the Action bar and above the bottom safe area, never over the primary action.

## Tokens

Additions in `packages/app-suite/src/tokens.css` (mirrored in [tokens.json]({{ site.baseurl }}{% link docs/developers/design/tokens.json %})):

| Token | Value | Job |
|---|---|---|
| `actionbar-h` | 56px | The Action bar's height, not counting the bottom safe area |
| `text-input-touch` | 16px | The minimum font size of an input under `(pointer: coarse)` |
| `safe-top`, `safe-right`, `safe-bottom`, `safe-left` | `env(safe-area-inset-*, 0px)` | The device's safe-area insets (notch, home indicator). They are non-zero only when the page has `viewport-fit=cover` |

Existing tokens that this layer leans on: `row-touch` (44 px), `space-4` (phone padding), `radius-lg` (a Sheet's top
corners), `shadow-overlay` (Sheets), `overlay` (the scrim behind a Sheet).

## The installed app on a phone

- Atlas installs as one app (`packages/app-suite/public/manifest.webmanifest`, `display: standalone`). A phone-first web app must:
  - have `<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">` in its `index.html`
  - honour the `safe-*` insets in the AppBar (top) and the Action bar (bottom)
  - never rely on the browser's address bar for navigation (every screen reachable from in-app navigation; the system back works)
- **The tab strip** ([Tabs]({{ site.baseurl }}{% link docs/developers/design/components/Tabs.md %})) is for laptops. Below 768 px the installed app shows no tab strip (see *Gaps*).
- A phone-first app should have a manifest **shortcut** (long-press on the home-screen icon) to its main task.

## Motion, loading and failure on a phone

- The same rules as `ui_design.md` (Loading and failure). Skeletons for known shapes, one spinner beside the action, and a
  failure confined to its region with **Try again**.
- Mobile networks drop. An action that saves (an answer) retries once on its own and then says so in one line, keeping the
  person's input. Never lose typed text because a request failed.
- Sheets slide in 200 ms and do not slide under `prefers-reduced-motion`.

## Testing a `touch` or `phone-first` app

Run these in Playwright (Chromium is installed in the dev environment; `require('playwright')`) or by hand in the browser's
device mode. Record the viewports in the app's `AGENTS.md`.

| Viewport | Why |
|---|---|
| 360×780, `hasTouch`, `isMobile` | the smallest common Android |
| 390×844, `hasTouch`, `isMobile` | a common iPhone |
| 844×390, `hasTouch`, `isMobile` | a landscape phone (the height is the constraint) |
| 768×1024, `hasTouch` | a portrait tablet |
| 1920×950 | the laptop target from `ui_design.md` |

**Check on each:**
- [ ] no horizontal page scroll
- [ ] every interactive element ≥ 44×44 under touch
- [ ] the primary action is visible without scrolling on focus screens
- [ ] with the keyboard simulated (set the viewport height to 420), the focused field and the Action bar are visible
- [ ] every command is reachable without hover and without right-click
- [ ] text ≥ 12 px, and inputs ≥ 16 px
- [ ] the screen reader order matches the visual order
- [ ] with reduced motion, nothing moves

## What the shell does today (baseline)

- `packages/app-suite/src/style.css`:
  - under `(pointer: coarse)`, tree rows and panel/popup buttons get `row-touch`
  - below 768 px the breadcrumb shows its last crumb, the app name and the ⌘K hint are hidden, and `.atlas-measure` pads
    `space-4`
- `packages/app-suite/src/preferences.ts`: the Inspector docks from 1200 px and opens by default from 1600 px.
- Help and the agent window are full-screen sheets on a phone (`components/Help.md`, `components/Assistant.md`).
- The learning `DropBoard` supports tap-to-place.

## Gaps (fix them in the first change that needs them, and update this list)

- [x] **The installed app's tab strip** (`TabHost.tsx` `isTabHost`) is off below 768 px (done in #220).
- [x] **Action bar, Sheet and FocusTask exist** (`packages/app-suite/src/mobile/`, #220). `SuiteModal` is a full-screen sheet below 768 px (CSS only).
- [ ] **`viewport-fit=cover`** is in Study and Languages' `index.html`. Each app adds it when it adopts `touch`, so the `safe-*` tokens are live there; the others still read 0.
- [x] **Tables** become cards below 768 px: `UnitStatsTable` and the shared admin tables (`data-label`, ARIA roles kept), #220.
- [ ] **The manifest shortcuts** Map, Workspace and Search point at older app paths (`/apps/ws-browser/`, `/apps/indexer/`); the Languages shortcut (`/apps/lang/?start=practice`) is current (#221).
- [x] **AppBar and DocHeader buttons under touch** (settled in #220): under `(max-width: 767px), (pointer: coarse)` the bar grows from 40 to 48 px (`--appbar-h`), and
      the AppBar, breadcrumb, DocHeader and panel buttons are at least 44 px (`style.css`, last block). Measured with Playwright (`hasTouch`) on Languages at 360×780, 390×844,
      844×390 and 768×1024: no interactive element is under 44×44 (a checkbox counts by its label). On a mouse the 28 px chrome is unchanged.
- [x] **`UnitStatsTable` links** are fingertip-sized in the card layout.
- [x] **A Sheet closes itself under React StrictMode** (found in #220: the first cleanup's `history.back()` fired its `popstate` after the effect set up again). The entry is now given back a tick later.
- [x] **The navigator drawer was open over the page on a first visit to a phone**: `loadPreferences` now starts it closed below 768 px (a saved choice still wins).

## Adopting it

An app that moves to `touch` or `phone-first`:
1. Says so in its `AGENTS.md` (the level and the test viewports above). When the catalog gains `web.targets` (an array of
   `laptop`, `tablet`, `phone`), it sets it there too, and the launcher can use it on a phone to put those apps first.
2. Uses the shell's regions and the components above, with no private versions. A missing piece goes into `packages/app-suite`
   with its spec in `components/`.
3. Fixes the gaps it touches, in `packages/app-suite`, so the next app inherits the fix.
4. Has help pages that do not assume a mouse ("tap or click", "long-press or right-click").
