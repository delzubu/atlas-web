---
description: Update the Atlas documentation site and changelog for new Core, Hobby and Work versions.
---

# Update release documentation

Run this command when an Atlas module publishes a release or records a new version on its default branch. If no module is specified, check all three. The result is a single site update with the changelog, affected documentation pages and the new version checkpoints published together.

## 1. Establish the baseline

- Work in the `atlas-web` repository, normally the `web/` folder beside `core/`, `hobby/` and `work/`. Read the site’s `AGENTS.md`, Core’s `AGENTS.md` and the publication guidance in `core/docs/design/product-brandbook.md` (§7 and §12). Read each affected module’s and app’s `AGENTS.md` before using its documentation.
- Open [release-state.json](release-state.json). Each entry records the most recent module version covered by the site, the exact Git commit reviewed, and the tag if there is one. Verify that the version agrees with the newest exact entry for that module in `docs/changelog/YYYY-MM-DD.md`. Treat a mismatch as a discrepancy to resolve from repository history before advancing the checkpoint.
- Confirm that the three source repositories match the repository names in the state file. Fetch each source’s default branch and tags without checking out, pulling into or changing the user’s source working tree. For the usual sibling layout, run these independently from `web/`.

```powershell
git -C ../core fetch origin --tags
git -C ../hobby fetch origin --tags
git -C ../work fetch origin --tags
```

Use `origin/main` as the published branch in each module. If a source repository or issue is inaccessible, report the missing access. Keep the affected checkpoint fixed while completing any independently verifiable module updates.

## 2. Find version boundaries

| Module | Authoritative version field | Module tag pattern |
| --- | --- | --- |
| Core | `VERSION` | `core-vX.Y.Z` |
| Hobby | `plugins.edition.version` in `plugins/instance.json` | `vX.Y.Z` |
| Work | `plugins.edition.version` in `plugins/instance.json` | `vX.Y.Z` |

For each module, list its module tags and walk the version-file commits on `origin/main` after the recorded commit, in chronological order. Example commands for Core and Hobby follow. Use the Hobby commands with `../work` for Work.

```powershell
git -C ../core tag --list 'core-v[0-9]*' --sort=version:refname
git -C ../core log --first-parent --reverse --format='%H %cs %s' '<recorded-commit>..origin/main' -- VERSION
git -C ../hobby tag --list 'v[0-9]*' --sort=version:refname
git -C ../hobby log --first-parent --reverse --format='%H %cs %s' '<recorded-commit>..origin/main' -- plugins/instance.json
```

Replace `<recorded-commit>` with the checkpoint. Read the module’s version field at each candidate commit or tag and verify it against the label. For editions, extract only `plugins.edition.version` from the JSON. Core’s npm package tags and Core tags inherited by an edition are outside the edition’s version history.

- Process each newly recorded module version in order, including intermediate versions. Prefer the module’s matching release tag when it exists. Use the published version-bump commit when it is the only boundary for that version.
- Check that the previous commit is an ancestor of the new boundary with `git merge-base --is-ancestor <previous> <current>`. Resolve gaps, rewritten history or unexpected tag targets before moving the checkpoint for that module.
- If a tag appears for a previously documented version, compare the recorded commit with the tagged commit. Update the same entry and any affected pages if the tag includes additional work, and record the tagged boundary.
- An unchanged version keeps its existing checkpoint even when `origin/main` has advanced. This preserves the intervening commits for the next release. If all versions and documented tag boundaries are unchanged, report the versions checked and finish without a site commit.

## 3. Review the changes and their intent

Use the previous version boundary and each new boundary from the same repository. Repeat for every intervening version.

```powershell
git -C ../core log --format=fuller '<previous>..<current>'
git -C ../core diff --name-status --find-renames '<previous>..<current>'
git -C ../core diff '<previous>..<current>' -- <relevant-path>
```

- Follow issue and pull request numbers in the commit messages and merge history. Use `gh issue view <number> --repo <owner/repo>` and `gh pr view <number> --repo <owner/repo>` to read the rationale, acceptance criteria and any linked or renamed issues. Use the named repository for cross-repository references. Search GitHub issues by the affected feature or pull request title if the commits lack a reference.
- Confirm what actually changed from the diff, tests and documentation at the target version. Use `git show <current>:<path>` or another read-only view of the target ref, especially if the source checkout contains local edits or newer changes. A closed issue or a newer file on `origin/main` may describe work outside the release being documented.
- Summarise what users and operators can do after the release. Leave private deployment identifiers, credentials and confidential Work data out of the public documentation. Link issues, pull requests or comparisons where helpful, with link text that explains their relevance.

## 4. Update the site

Review each surface below against the commit ranges, including cross-module dependencies. Change the pages whose content, examples or navigation are affected.

| Site area | Sources to check | What to update |
| --- | --- | --- |
| `docs/apps/` | App and API `AGENTS.md`, `README.md`, `apps/<app>/web/help/`, shared shell help, app catalog and relevant specs | App availability, purpose, labels, workflows, access, agent capabilities and supporting playbook links. Update bundle listings when apps are added, moved or retired. Preserve existing public URLs or provide redirects when a move is necessary. |
| `docs/playbooks/` | `library/playbooks/`, each playbook’s `SKILL.md`, stage files and library documentation | Core, Hobby and Work catalogs, inputs, outcomes and links from relevant apps. |
| `docs/playbooks/templates.md` | `library/templates/*/template.yml` and the template’s starter docs | Templates, included materials, setup playbooks and related app links. |
| `docs/developers/` | Root and package setup guides, `INSTALL.md`, `devops/DEPLOY.md`, manifests, scripts, shared specs and release tooling | Development setup, supported tools, package resolution, deploy and settings workflows, module boundaries and API or SDK guidance. |
| `index.md` and `docs/getting-started.md` | Launcher, sign-in and suite-wide help | Suite positioning, first-use steps and navigation affected by the release. |
| `docs/changelog.md` and `docs/changelog/YYYY-MM-DD.md` | The version ranges and issues reviewed above | One index row per release date and one detail entry per changed module version. |

Use the release tag’s date or the version-bump commit date when there is no tag. Add the release to `docs/changelog/YYYY-MM-DD.md`, creating the page from an existing day’s structure if needed. Use the quoted `"YYYY.MM.DD"` as the title, `parent: Changelog`, `grand_parent: Home`, and `permalink: /docs/changelog/YYYY-MM-DD/`. Set `nav_order` to the negative integer formed from the date, such as `-20261124`, so the sidebar lists the newest dates first. Link back to the index and give the date heading the `.release-date` class. Give each exact version a `## Module X.Y.Z` heading with the `.release-module` class, one summary paragraph and a bullet list. List each module’s releases newest first and retain the other entries on that day.

Add or update the date’s row in `docs/changelog.md`. Keep dates newest first, link the displayed date to the daily page with the Jekyll link pattern used by the existing rows, and summarise the day’s releases in one sentence. In **Versions released**, put each module on its own line using `<br>`. Show each major/minor series once per module, newest first. Use `X.Y.0` when that exact version appears on the daily page and `X.Y.x` for a series represented by patches only on that day. Separate multiple series for one module with commas.

After all affected pages and changelog entries are updated, advance only the completed modules in `release-state.json`. Record the newest documented version, the full commit SHA and the matching tag or `null`. Keep changes for each version traceable in the changelog even when several versions share one site update. If a module’s review is incomplete, leave its checkpoint in place and report what remains.

## 5. Verify and publish

1. From `web/`, run `bundle exec jekyll build` and `git diff --check`. Verify changed page links, anchors, the changelog’s date and version order, and the checkpoint values against the Git refs. If Ruby is unavailable locally, run the Jekyll build in a Ruby 3.x Docker container using the site’s `Gemfile`. Preview rendered layout or navigation changes.
2. Review the diff for unsupported claims, private content, unrelated files and unintended deletions. Stage the affected site pages, changelog and release state together. Check the staged diff and commit the update. Publish to `origin/main` by fast-forward under the site’s Git rules. Keep the source repositories and their release tags untouched.
3. Check GitHub Pages for the pushed commit with `gh api repos/delzubu/atlas-web/pages/builds/latest`. Verify the live changelog and affected pages. Report the module versions processed, the site commit, checks run and any unresolved release-review gaps.
