---
title: Design
parent: Developers
nav_order: 5
has_children: true
has_toc: false
permalink: /docs/developers/design/
---

# Atlas design system

Atlas — The Light Factory is a workshop for bespoke knowledge work. Its design system makes a collection of specialised instruments feel like one coherent environment for people and agents. Content takes priority over interface chrome; the work persists beyond an individual conversation or model provider.

## Read the guides

| Guide | What it covers |
| --- | --- |
| [Brand book]({{ site.baseurl }}{% link docs/developers/design/product-brandbook.md %}) | Product positioning, voice, visual identity, approved logo assets and palette. |
| [UI design language]({{ site.baseurl }}{% link docs/developers/design/ui_design.md %}) | Shared shell, layout, colour roles, typography, commands, loading and keyboard shortcuts. |
| [UX guide]({{ site.baseurl }}{% link docs/developers/design/ux.md %}) | User journeys, content focus, connected workflows, agent collaboration, recovery and review criteria. |
| [Touch and small screens]({{ site.baseurl }}{% link docs/developers/design/mobile.md %}) | Responsive layouts, touch targets, typing, mobile components and testing. |
| [Component reference]({{ site.baseurl }}{% link docs/developers/design/components/index.md %}) | Detailed anatomy and behaviour of shared controls and regions. |
| [Visual previews]({{ site.baseurl }}{% link docs/developers/design/previews.md %}) | Standalone HTML examples using the copied design tokens. |
| [Screenshot gallery]({{ site.baseurl }}{% link docs/developers/design/screenshots.md %}) | Historical and reference screenshots of the implemented UI. |

## How the guides fit together

The brand book owns identity and brand colours. The UI guide owns look and structure; the UX guide owns flow and behaviour. The mobile guide takes precedence for touch and screens below 1200 px. The [CSS tokens]({{ site.baseurl }}{% link docs/developers/design/tokens.css %}) and [machine-readable tokens]({{ site.baseurl }}{% link docs/developers/design/tokens.json %}) accompany this website edition.

![Knowledge base desktop example]({{ site.baseurl }}{% link docs/developers/design/screenshots/kb-after-1920.png %})

The source specifications remain authoritative in [atlas-core/docs/design](https://github.com/delzubu/atlas-core/blob/4a656981ba21c6139a3d3750e0cb76f8d706284a/docs/design). This website edition is a publication snapshot of commit [4a65698](https://github.com/delzubu/atlas-core/commit/4a656981ba21c6139a3d3750e0cb76f8d706284a), not a separate design authority. When changing a design rule, update the source first and refresh this copy. The [original design overview](https://github.com/delzubu/atlas-core/blob/4a656981ba21c6139a3d3750e0cb76f8d706284a/docs/design/README.md) records implementation status and historical measurements; the [migration proposal](https://github.com/delzubu/atlas-core/blob/4a656981ba21c6139a3d3750e0cb76f8d706284a/docs/design/proposal.md) explains the earlier redesign.

The [developer guide]({{ site.baseurl }}{% link docs/developers/index.md %}) describes the suite architecture and implementation workflow.
