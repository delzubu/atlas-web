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
| `docs/` | Content. One folder per section; `docs/apps/` holds one page per Atlas app (`parent: Apps`) |

## Brand and design

Follow `core/docs/design/product-brandbook.md` (§12 is the one source of the logo and palette) and the publication voice (§7). This is a publication, so §12.3 applies, not the product UI rules:

- Graphite text on White or Mist; Aubergine for headlines, the symbol and accents; Coral only for the curved underline and at most one other highlight per page. Links keep the product's interaction blue (`#2563eb`), never Aubergine or Coral.
- One light theme. No dark mode, no recolouring of the logo, no superseded blue marks. Sentence case in headings and labels.
- The logo files are copies. If the brand changes, copy the new files from `core/docs/design/assets/` and update the palette in `atlas.scss` from §12.2 in the same change.
- Verify current capability before making a feature claim; the brand book says so too. Describe apps from their own `AGENTS.md` and specs, not from memory.

## Writing pages

- Front matter: `title`, `nav_order`, and for children `parent: <title>`; a section index uses `has_children: true` and a `permalink`. Link with `{% link path.md %}` so a broken link fails the build.
- Content under `docs/` is documentation for readers, not instructions for agents.
- Never put credentials, instance URLs, tenant or account identifiers, or anything from the `.env` files or `secrets/` in a page. Confidential `work` edition content (Oppos CRM data) does not belong here.
- Do not hand-edit `_site/` or `.jekyll-cache/` (git-ignored, generated). `Gemfile.lock` is ignored too; the workflow resolves gems on each run.

## Checks

- Narrowest check: `bundle exec jekyll build` (fails on broken `{% link %}` tags and bad Sass). There is no CI; the Pages build log (Settings → Pages, or `gh api repos/delzubu/atlas-web/pages/builds/latest`) is the first sign of a broken push.
- There are no unit tests. For a visual change, run the preview and look at it.

## Git

Own repository (`origin` = `delzubu/atlas-web`, branch `main`). New sections or design changes go on a feature branch; page edits and fixes go straight to `main` in small commits. Don't push unless asked. A push to `main` publishes the site.
