---
title: Developers
nav_order: 4
has_children: true
permalink: /docs/developers/
has_toc: false
---

# Develop Atlas

Start with the project you want to change. Atlas Core provides sign-in, storage, several apps, and the shared packages. Optional editions build on Core and add apps to an Atlas instance.

Most app work starts in `apps/<app>/`. Python services live in `api/` and React and Vite web apps live in `web/`. Check the app's README and `AGENTS.md` for its commands and contracts.

1. [Set up a development environment]({{ site.baseurl }}{% link docs/developers/setup.md %})
2. [Find your way around the modules]({{ site.baseurl }}{% link docs/developers/modules.md %})
3. [Understand instance and edition deployment]({{ site.baseurl }}{% link docs/developers/deployment.md %})
4. [See the SDK documentation outline]({{ site.baseurl }}{% link docs/developers/sdk.md %})
