---
title: Changelog
parent: Home
nav_order: 1
permalink: /docs/changelog/
---

# Changelog
{: .release-title }

Core, Hobby and Work are versioned independently. The first entry for each module records its standalone baseline. Issue and pull request links may require repository access.

## 2026.11.24
{: .release-date }

### Core 1.1.1
{: .release-module }

Core 1.1.1 packages the Search integration introduced in 1.1.0 as a tagged release.

- Updated the versions of the shared Python and npm packages and Workspace.
- Published the matching App Suite and Catalog npm tags. [Core 1.1.1 tag](https://github.com/delzubu/atlas-core/tree/core-v1.1.1)

### Core 1.1.0
{: .release-module }

Atlas Search moved into Core. People and agents can search configured knowledge indexes from one place. This version also introduced a deployment settings provider and let operators select which edition apps to deploy.

- Added the Search web app, REST API and MCP tools for querying, refining results, checking index statistics and curating indexed knowledge. [Search integration](https://github.com/delzubu/atlas-core/pull/28)
- Connected Search to Look up, Workspace project home and Assistant. Added a staged progress display and database management in Search. [Search interface](https://github.com/delzubu/atlas-core/pull/29)
- Added an optional settings provider that populates instance environment files and records values learned during deployment. [Environment provider](https://github.com/delzubu/atlas-core/pull/25)
- Added per-instance app selection for edition deployments. [App selection](https://github.com/delzubu/atlas-core/pull/27)

## 2026.11.20
{: .release-date }

### Core 1.0.2
{: .release-module }

Hobby and Work can resolve the Core packages they need at build time. Each edition can publish independently and serve its apps from its own host.

- Added a resolver that uses the installed Core packages or fetches a published release, subject to the edition’s minimum version.
- Added a local release command for publishing versioned packages and tags.
- Updated edition web apps to request OAuth access for their own host and link back to the Atlas instance. [Compared with Core 1.0.1](https://github.com/delzubu/atlas-core/compare/core-v1.0.1...core-v1.0.2)

### Core 1.0.1
{: .release-module }

Compatible Core installations can be reused, and Workspace intake now requires an explicitly configured data store.

- Kept compatible installed Core packages during dependency setup.
- Made deployment validate the Workspace intake store explicitly. [Compared with Core 1.0.0](https://github.com/delzubu/atlas-core/compare/core-v1.0.0...core-v1.0.1)

### Core 1.0.0
{: .release-module }

Core reached its first major version as an independent prerequisite for Hobby and Work. The three repositories can be versioned and released separately. [Repository separation epic](https://github.com/delzubu/atlas-hobby/issues/261)

- Updated plugin metadata to refer to the owning repository.
- Made library ownership checks accept checkout folders with or without the `atlas-` prefix.

### Core 0.3.4
{: .release-module }

Edition redeployments now preserve the resource name prefix when a local checkout is renamed.

- Reused the prefix of the existing Function App when no explicit prefix was configured. [Compared with Core 0.3.3](https://github.com/delzubu/atlas-core/compare/core-v0.3.3...core-v0.3.4)

### Core 0.3.3
{: .release-module }

Core fixed two problems with its Windows release and deployment tooling.

- Found `npm.cmd` through the Windows command search path when publishing npm tags.
- Corrected the backend assembly script’s PowerShell parameter block. [Compared with Core 0.3.2](https://github.com/delzubu/atlas-core/compare/core-v0.3.2...core-v0.3.3)

## 2026.11.19
{: .release-date }

### Core 0.3.2
{: .release-module }

Core’s initial standalone packaging made release artifacts available to editions for setup, testing and deployment.

- Updated the installer, plugin generator and deployment assembly to consume published Core packages. [Published-artifact deployment issue](https://github.com/delzubu/atlas-hobby/issues/362)
- Updated Python wheel and npm packaging for release-based installation.

### Core 0.3.1
{: .release-module }

The first standalone Core baseline brought together Atlas sign-in, project files, scheduling, knowledge bases and Assistant. [Repository extraction issue](https://github.com/delzubu/atlas-hobby/issues/270)

- Included Identity, Workspace, Planner, KB Explorer and the shared Assistant panel.
- Adjusted the release tooling to build wheels from the correct package paths and exclude generated build output from npm packages.

### Hobby 0.45.0
{: .release-module }

Hobby began as a separate repository for worldbuilding, tabletop campaigns and personal learning. This is its first recorded standalone edition version. [Repository separation epic](https://github.com/delzubu/atlas-hobby/issues/261)

- Carried over Greyhawk Map, Mapper and NPC for maps and campaign characters.
- Included Study and Languages, along with worldbuilding and learning playbooks and templates.

### Work 0.45.0
{: .release-module }

Work began as a separate repository for modelling, stakeholder engagement and opportunity review. This is its first recorded standalone edition version. [Repository extraction issue](https://github.com/delzubu/atlas-hobby/issues/270)

- Carried over Causal, UML, Stakeholders and Oppos.
- Included Work playbooks and templates for activities such as pursuits, proposals and meetings.
