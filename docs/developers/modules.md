---
title: Modules
parent: Developers
nav_order: 2
---

# Modules

| Project | What it contains |
| --- | --- |
| Atlas Core | Identity, Workspace, Planner, Knowledge base, Search, Assistant, and the shared libraries and deployment tooling. |
| Atlas Hobby | Map, Mapper, Study, Languages, and NPC, plus shared learning components. |
| Atlas Work | Causal, UML, Stakeholders, and Oppos. |

## Shared packages

- The catalog describes apps and their capabilities. The Python and web layers read from the same catalog.
- `atlas-core` provides shared Python contracts and utilities, including token verification and `atlas://` link handling.
- `atlas-docapp` helps document apps store their documents in Workspace and expose operations through REST and MCP.
- `@atlas/app-suite` provides shared React components, sign-in, and navigation for the web apps.

## Dependencies

Hobby and Work resolve Core as a package dependency. Each edition declares a minimum supported Core version in `testing/suites.json`. No sibling Core checkout is needed, and the editions do not track a branch of Core. Core and the two editions release and deploy independently.

Apps use shared packages for common code. An edition can discover another edition’s optional capabilities through the instance’s module registry at runtime. The edition projects have no build-time dependency on one another.
