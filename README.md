# atlas-web

The Atlas documentation site, published with GitHub Pages: Jekyll with the [Just the Docs](https://just-the-docs.com) theme, coloured with the Atlas brand.

Preview locally (needs Ruby 3.x and Bundler): `bundle install && bundle exec jekyll serve`, then open http://localhost:4000/atlas-web/.

Pages deploy from `main` through `.github/workflows/pages.yml`. In the repository settings, set Pages → Source to **GitHub Actions** once. See `AGENTS.md` for how the site is organised.
