---
title: Workspace
parent: Core
grand_parent: Apps
nav_order: 6
---

# Workspace

Workspace gives people and agents a shared place to work on project files. Browse folders, edit Markdown, and review changes with comments and file history. You can also bring in source material from documents and web links. Planner plans, Mapper maps, and other Atlas documents can live alongside the files they draw on.

## How to use Workspace

1. Open an existing project from the welcome screen, create an empty one or choose **New project from template**. The project home shows its description, references, prompts and recent activity.
2. Use the file tree to open a document. Edit Markdown in the visual or source editor, or open a plan, map or model in its own app.
3. Use comments to review work and examine file history or change sets to see what changed. Use **Look up** for the project’s configured knowledge sources.

Upload files and use intake for supported Office documents, PDFs, saved email or web links. Intake keeps the original alongside derived material for review and reuse. The local sync tools support a checkout when you need to work on files outside the browser.

## Work from a local checkout

### Why

A checkout is a folder on your machine that mirrors a workspace. You can use any editor, run scripts over the files, work offline, and then send your changes
back as one commit. It follows the model of a git repository (but is not git): a hidden `.ws/` folder in the checkout keeps the local state.

### The commands

```
ws-tool git clone quarterly-review/main     # make the folder
cd quarterly-review
ws-tool git status                          # what changed here, and upstream
ws-tool git add -A
ws-tool git commit -m "Update the summary"  # a local commit
ws-tool git push                            # send it to the workspace
ws-tool git pull                            # bring in what others changed
ws-tool git diff HEAD~1 -- notes.md
ws-tool git log --upstream
```

In Workspace your pushed work appears as ordinary changes, with your name, in the Changes tab.

### What is and is not synced

| Synced | Not synced |
| --- | --- |
| Every file in the workspace, including system folders (`.plans/` and the other apps' document folders) | The checkout's own `.ws/` folder |
| Office files with their extraction folders (`.brief.pptx.derived/`) | Anything matched by `sync.ignore` in `workspace.yml` (patterns such as `*.tmp`, `scratch/`, `.DS_Store`) |

Do not edit the files inside a system folder by hand in a checkout: they belong to the documents' apps, and a hand edit can leave a document unreadable.

### Office files in a checkout

`ws-tool git commit` ingests Office files you staged (it creates or refreshes their extraction folders and stages them too), so a push brings the text and pictures along. The same step
is available alone as `ws-tool derive`, and `ws-tool intake <URL>` inside a checkout refreshes the file there (Intake).

### Conflicts

Changes that clash with what someone else did are reported by `ws-tool git status` and `ws-tool git pull`. Resolve them in your editor, add the file and commit again; `ws-tool git -h` lists the options.

## Move older workspace documents into system folders

### What changes

| Before | After |
| --- | --- |
| `plans/<name>/`, any other folder holding a plan | `.plans/<name>/` |
| any folder holding another app's document (its `about.json` says which) | that app's system folder, such as `.maps/<name>/` |
| `brief.pptx.derived/` | `.brief.pptx.derived/` |

Documents are found by their description file wherever they are, and both layouts of extraction folders are read, so old workspaces work as they are.
Moving them just makes every project look the same and tidies the file tree.

### The command

From a command prompt on a computer with `ws-tool` set up:

```
ws-tool --project quarterly-review --ref main migrate-layout --dry-run
ws-tool --project quarterly-review --ref main migrate-layout
```

`--dry-run` prints what would move and which links would be rewritten, and changes nothing. Look at it first.

### What it does

- Moves each document folder (a plan, or another app's document) into its app's system folder (adding `-2` to a name that is already taken there).
- Moves each Office extraction folder to its dot-named twin, unless one already exists there (then it is left alone and reported).
- Rewrites `atlas://workspace/<project>/<ref>/<old path>` links to the moved folders in **every text file of the project**: `workspace.yml` References, each document's own links, notes. A longer name (`plans/townsend`) is never confused with `plans/town`.
- Does all of it as **one change**, so Changes → Undo puts everything back.

### What it cannot do

- It cannot rewrite links to the moved folders **from other projects**. The report lists the old and new addresses; update those projects' References by hand.
- Relative Markdown links (`../plans/town/…`) are not rewritten. Links to documents are normally `atlas://` links, which are.
- It does not move files that are not documents. A folder you made yourself stays where it is.

### After

The maps, plans and models appear in the project home as before; their folders are in the system folders, hidden unless **Show system files** is on. Anyone with a local checkout should `ws-tool git pull`.

## Playbooks supporting work

[Browse the playbooks]({{ site.baseurl }}{% link docs/playbooks/index.md %}) for a workflow, or start the workspace from a [project template]({{ site.baseurl }}{% link docs/playbooks/templates.md %}). A template can include starter files and a setup playbook.
