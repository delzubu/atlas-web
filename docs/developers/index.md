---
title: Developers
nav_order: 6
has_children: true
permalink: /docs/developers/
has_toc: false
---

# Develop Atlas

Start with the project you want to change. Atlas Core provides sign-in, storage, several apps, and the shared packages. Hobby and Work use released Core packages and each can be developed from its own checkout. You only need Core source locally when changing Core itself or deliberately testing an edition against a local Core change.

Most app work starts in `apps/<app>/`. Python services live in `api/` and React and Vite web apps live in `web/`. Check the app's README and `AGENTS.md` for its commands and contracts.

1. [Set up a development environment]({{ site.baseurl }}{% link docs/developers/setup.md %})
2. [Find your way around the modules]({{ site.baseurl }}{% link docs/developers/modules.md %})
3. [Understand instance and edition deployment]({{ site.baseurl }}{% link docs/developers/deployment.md %})
4. [See the SDK documentation outline]({{ site.baseurl }}{% link docs/developers/sdk.md %})
