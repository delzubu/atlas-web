# AGENTS.md

Guidance for agentic systems working in `web/` (atlas-web). Rules that apply everywhere are in `core/AGENTS.md`.

## Purpose

The public documentation site of Atlas, published with GitHub Pages from `github.com/delzubu/atlas-web`. It is a static Jekyll site: Markdown pages, no app code, no backend, no secrets. It is not part of the Atlas suite's runtime and no Atlas package depends on it.

## Stack

- Jekyll with the **Just the Docs** theme, loaded as `remote_theme: just-the-docs/just-the-docs@<release>` (pinned in `_config.yml`; the `jekyll-remote-theme` plugin is on GitHub's allowed list). Navigation, search and layout come from the theme; do not copy its layouts here unless a change cannot be made any other way.
- Built by GitHub Pages' own **branch-based (legacy) build** from `main`, root folder (Settings → Pages → Deploy from a branch). **GitHub Actions is not used** (the owner has none available): don't add a workflow. That build runs the `github-pages` gem's Jekyll 3.x, so only its allowed plugins work; keep `Gemfile` in line with it.
- Live at https://delzubu.github.io/atlas-web/ . Local preview: `bundle install && bundle exec jekyll serve` (Ruby 3.x). Ruby is not installed on the owner's machine by default, so a change may be unbuilt: say so rather than claim it renders.

## Layout

| Path | What |
|---|---|
| `_config.yml` | Site title, `url`/`baseurl` (`/atlas-web` while it is a project page; `""` with a custom domain), theme, search, footer, edit-link settings |
| `_sass/color_schemes/atlas.scss` | The `atlas` colour scheme: brand palette mapped onto the theme's variables |
| `_sass/custom/custom.scss` | The few overrides: system sans, Aubergine headings, one Coral underline per page, focus ring |
| `_includes/head_custom.html` | Favicon and theme colour |
| `assets/images/`, `favicon.svg` | Logo files, copied from `core/docs/design/assets/` |
| `index.md`, `404.md` | Home page and not-found page |
| `docs/changelog.md`, `docs/changelog/` | Release-date index under Home and one detail page per date |
| `docs/` | Content. `docs/apps/` holds bundle and app guides. `docs/playbooks/` holds the library guide and template catalog. `docs/developers/` covers development and deployment |

## Brand and design

Follow `core/docs/design/product-brandbook.md` (§12 is the one source of the logo and palette) and the publication voice (§7). This is a publication, so §12.3 applies, not the product UI rules:

- Graphite text on White or Mist; Aubergine for headlines, the symbol and accents; Coral only for the curved underline and at most one other highlight per page. Links keep the product's interaction blue (`#2563eb`), never Aubergine or Coral.
- One light theme. No dark mode, no recolouring of the logo, no superseded blue marks. Sentence case in headings and labels.
- The logo files are copies. If the brand changes, copy the new files from `core/docs/design/assets/` and update the palette in `atlas.scss` from §12.2 in the same change.
- Verify current capability before making a feature claim; the brand book says so too. Describe apps from their own `AGENTS.md` and specs, not from memory.

## Writing pages

- Keep sample project files in the repository's `examples/` directory, excluded from Jekyll publication. Link to their GitHub folder (`tree/main/examples/...`), not to rendered sample pages.

- Home introduces the product and its value; Getting started provides separate paths for invited users, evaluators and operators. Describe model independence without claiming universal compatibility or pooled subscription allowances. Use actual reference screenshots with accurate captions; label illustrative examples.
- Articles are a product-manager editorial blog: publish useful insights as they arise, with flexible depth and format. Keep existing article structure and style; do not turn the collection into a mandatory curriculum or duplicate the app reference.
- Apps landing pages support task-oriented discovery; retain repository/module navigation for reference. Product availability and deployment packaging are distinct.
- The Design section is a human-readable publication of Core's canonical guides. Preserve rule identifiers, source revision, assets and status qualifications. Refresh it when the source changes.

- Put task walkthroughs in the relevant app guide under `docs/apps/`. In-app help in the source projects describes controls, screens and states and links to these walkthroughs. Keep published guide paths and anchors stable when app help links to them.

- Front matter: `title`, `nav_order`, and for children `parent: <title>`; a section index uses `has_children: true` and a `permalink`. Link with `{{ site.baseurl }}{% link path.md %}` so Jekyll 3 validates the target and includes the project site prefix.
- Content under `docs/` is documentation for readers, not instructions for agents.
- Never put credentials, instance URLs, tenant or account identifiers, or anything from the `.env` files or `secrets/` in a page. Confidential `work` edition content (Oppos CRM data) does not belong here.
- Do not hand-edit `_site/` or `.jekyll-cache/` (git-ignored, generated). `Gemfile.lock` is ignored too: Pages resolves its own gems.

## Maintain the changelog

On every new Core, Hobby or Work version, run the [release documentation command](.commands/update-release-docs.md). It covers the changelog and all affected site areas, including apps, playbooks, templates and the developer guide. The [release state](.commands/release-state.json) records the last documented version and exact Git boundary for each module. Update it with the corresponding site changes in the same commit.

1. Check Core, Hobby and Work independently. Add one entry when a module's own version number changes. Core's version is in `VERSION` and its release labels are `core-vX.Y.Z`. Hobby and Work use their root `VERSION` files and their own `vX.Y.Z` labels. Core package tags, inherited Core tags in an edition, and an unchanged edition's Core dependency updates do not create extra entries.
2. Locate the previous and current version labels in that module's repository and review the full commit range with `git log <previous>..<current>` and `git diff <previous>..<current>`. Use the committed version bump as the boundary when there is no tag. For a first standalone version, use the initial snapshot as the baseline. If a tag later appears for an already documented version, review through the tagged commit and update that entry.
3. Read GitHub issues referenced by the commits and pull requests to understand the goal and the result. Follow migrated issues when relevant. Describe what shipped based on the commit range, since an issue may include work planned for a later version. Link relevant issues, pull requests or comparisons where they help readers. Keep private operational details and confidential data out of the public page.
4. Put all entries for a date in `docs/changelog/YYYY-MM-DD.md`. Use a separate `## Module X.Y.Z` entry for every exact version. Add or update that date’s row in the index, with a one-sentence summary and patches rolled up by module and major/minor series. The release documentation command gives the page and row formats.
5. Run the site build and verify the changelog renders under Home. Check links in both directions, newest-first dates, and that each index row reflects its detail page.

## Checks

- Narrowest check: `bundle exec jekyll build` (fails on broken `{% link %}` tags and bad Sass). There is no CI; the Pages build log (Settings → Pages, or `gh api repos/delzubu/atlas-web/pages/builds/latest`) is the first sign of a broken push.
- There are no unit tests. For a visual change, run the preview and look at it.

## Git

Own repository (`origin` = `delzubu/atlas-web`, branch `main`). For every update, run the relevant checks, commit the changes, and push them to `origin/main` as part of the task. New sections and design changes start on a feature branch and land by fast-forward. Page edits and fixes can go straight to `main` in small commits. A push to `main` publishes the site.
