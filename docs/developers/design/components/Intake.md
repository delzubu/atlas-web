---
title: Intake
parent: Component reference
grand_parent: Design
nav_order: 13
nav_exclude: true
---

# Intake: Upload files, Add link, the item panel and status chips

[Component reference]({{ site.baseurl }}{% link docs/developers/design/components/index.md %}) · [Source specification](https://github.com/delzubu/atlas-core/blob/4a656981ba21c6139a3d3750e0cb76f8d706284a/docs/design/components/Intake.md)

How a person adds files and links to a Workspace and sees what became of them. The rule behind it, **save now, process later**, is in
[`docs/specs/intake.md`](https://github.com/delzubu/atlas-core/blob/4a656981ba21c6139a3d3750e0cb76f8d706284a/docs/specs/intake.md); this page is the screen. **Status: implemented** (`apps/workspace/web/src/components/Intake/`, epic #238, #248).

Both dialogs follow principle 2 of [ui_design.md]({{ site.baseurl }}{% link docs/developers/design/ui_design.md %}), **lean by default**: what the person needs in the moment and one primary action. Choices that rarely
matter (how a dropped link is kept, the server's limits, processing details) are not on the dialog; they have sensible defaults and live on the item's page.

## Upload files (dialog)
```
 Upload files                                          ×
 Folder  [ inbox                      ]   ← existing folders suggested; a new name is a new folder
 ┌──────────────────────────────────────────────────┐
 │        Drop files here, or [ Choose files… ]     │
 └──────────────────────────────────────────────────┘
 brief.pptx                                 2 MB   ×
 notes.docx                                40 KB   ×
                                    [ Upload 2 files ] [ Cancel ]
```
- **Entries.** The navigator tool "Upload files", the folder menu "Upload files here…", the palette command [`workspace.upload`], files dropped on a folder in the tree, and
  Ctrl+V of files (or several addresses) outside any field. None of them opens the system file picker directly; the box's **Choose files…** does.
- **Folder** is a text field with the loaded folders as suggestions (`FolderField`). A name that is not one of them shows "New folder: it is made when you save."
- **The list** is one line per item: name, size (or "link"), and × to remove. After saving a line says *Saving…*, then *Saved*; processing is shown afterwards on the item's page and as a
  chip in the tree, never here.
- **Per-item failure.** Items are saved as one request but succeed or fail one by one. A failed line keeps its place with the reason under it, and only then does its name become an editable
  field (the usual cause is a name already taken). The same idempotency key is used for the dialog's lifetime, so a retry never duplicates what was saved. When everything is saved the
  dialog closes by itself.
- **No replacing.** A name that is taken fails that line; it is never overwritten (`expected_revision: null` on the server).
- **Addresses dropped on the box** become link lines kept the suggested way; to choose how a link is kept, use Add link.
- **Limits** are checked before anything is sent: a file over the upload limit says so on its line and blocks Upload; more items than one request takes says so once. Files over 8 MB use bounded authenticated chunks before the save request; the server verifies the full hash and size before publication (the page never holds them whole in memory).
- **No drag-only paths** ([mobile.md]({{ site.baseurl }}{% link docs/developers/design/mobile.md %})): *Choose files…* does everything a drop does. Under `(pointer: coarse)` controls are 44 px and text 16 px; `SuiteModal` is a full-screen sheet on a phone.

## Add link (dialog)
```
 Add link                                              ×
 Address    [ https://contoso.sharepoint.com/…            ]
 Folder     [ inbox                                       ]
 File name  [ Q3 brief.pdf                                ]
 Keep       [ Keep preview updated ▾ ]  Keeps a copy of the text and preview…
                                         [ Add link ] [ Cancel ]
 Saved as inbox/Q3 brief.pdf.url. The preview is made afterwards; the link works at once.
```
- **Entries.** The navigator tool, "Add link here…" in a folder menu, the palette command [`workspace.add-link`], one address dropped on a folder in the tree, and Ctrl+V of one address
  outside any field (the address is filled in).
- **Keep** appears only when more than one way is supported for that address; the suggestion is selected. An address that is not allowed says why and disables Add link.
- Nothing is fetched while saving: an address that cannot be reached still becomes a link.

## Item panel (`IntakePanel`)
A bordered block under the link bar of a link or Office file: a **status chip and a sentence** ("Waiting its turn in the background…", "Up to date.", "The last attempt did not work. The source could not
be reached. The earlier preview is kept."), the **questions** a run asked (with the answers as buttons, only for the person who may answer), the facts (*Kept as*, *Refresh*, *Added*), and
**Refresh**, **Refresh with options…** and **Cancel**. Read-only access sees the status and a sentence saying why there are no buttons.
- The panel looks again at the server **only while a run is going** (750 ms, easing to 5 s) and once when the tab is returned to (`useOnReturn`); an idle page makes no requests.
- When a run finishes the page reloads the text and pictures it made.
- On a server without intake the panel steps aside and the old banners with the command to run stay.

## Status chip (`StatusChip`)
A word in a small outlined pill: **Created, Processing, Needs input, Ready, Failed, Deferred, Cancelled**. Colour only reinforces (muted for waiting, accent for needs input, danger for failed); the
word always carries the meaning. In the tree it sits in the row's `trail` and **Ready is not shown** (it is the normal case).

## Voice
Verb-first buttons ("Add 3 items", "Refresh", "Keep a preview…"), sentence case, numerals for counts, plain words for states; see [Forms]({{ site.baseurl }}{% link docs/developers/design/components/Forms.md %}).
