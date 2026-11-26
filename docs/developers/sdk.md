---
title: SDK and integrations
parent: Developers
nav_order: 4
---

# SDK and integrations

Atlas supports two distinct extension routes. Integrate an existing agent or client through the exposed MCP or REST contracts, or develop an application using the shared packages and document model.

## Start with a small integration

The [first integration walkthrough]({{ site.baseurl }}{% link docs/developers/integration-example.md %}) shows how to connect a compatible agent, inspect permissions, read a file and make a bounded, reviewable change. It does not require building another Atlas app.

## Build on the shared packages

| Area | Starting point |
| --- | --- |
| Python services | [atlas-core](https://github.com/delzubu/atlas-core/tree/main/packages/atlas-core): shared contracts, authentication and links |
| Document apps | [atlas-docapp](https://github.com/delzubu/atlas-core/tree/main/packages/atlas-docapp): workspace-backed documents, revisions and API support |
| Web apps | [app-suite](https://github.com/delzubu/atlas-core/tree/main/packages/app-suite): React shell, sign-in and shared components |
| Capability registration | [catalog](https://github.com/delzubu/atlas-core/tree/main/packages/catalog): app and service descriptions |
| Agent integrations | [plugin source](https://github.com/delzubu/atlas-core/tree/main/plugins) and each app's REST/MCP implementation |

Read [Modules]({{ site.baseurl }}{% link docs/developers/modules.md %}) for dependency boundaries and [Design]({{ site.baseurl }}{% link docs/developers/design/index.md %}) for UX and component rules.

The package source, README files and [contracts](https://github.com/delzubu/atlas-core/tree/main/docs/specs) remain authoritative. This is an entry point to the SDK, not a complete API reference.
