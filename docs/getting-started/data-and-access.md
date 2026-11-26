---
title: Your data and agent access
parent: Getting started
nav_order: 2
---

# Your data and agent access

Atlas separates the working environment from the AI service. This provides continuity across compatible clients, but it does not make information invisible to the agents you authorise.

## Where the work lives

Workspace files and revisions are stored in the instance's configured storage (Azure Blob Storage in the standard hosted deployment). Workspace-backed plans and other models live with the project. Knowledge bases may also use Git repositories; Search uses configured external databases and inference services. These stores have their own operational and retention requirements.

Switching between compatible AI providers does not move or recreate the saved Atlas artifacts. Conversation history is separate and may remain with the provider. A workspace checkout can export files for local work; see [Workspace]({{ site.baseurl }}{% link docs/apps/workspace.md %}) for the supported sync and export operations.

## What an agent can access

Connected agents act through Atlas APIs under authorised identities and scopes. Access also depends on app entitlements and applicable project grants.

| Role | Typical content capability |
| --- | --- |
| Reader | Inspect permitted content |
| Reviewer | Read and participate in supported review operations |
| Writer | Create and modify permitted content |
| Administrator | Manage access through eligible administration clients |

A role is not a grant to every workspace. MCP does not provide administrative authority. The detailed [authorization contract](https://github.com/delzubu/atlas-core/blob/main/docs/specs/authorization.md) defines the checks and exceptions.

A model provider can receive material retrieved by the agent, prompts, tool results or attachments. Atlas does not automatically transfer the entire workspace to every connected agent, but a permitted agent can request substantial content. The provider's processing, retention and training terms depend on that service and account. Confirm those terms before using sensitive material. Revocation is not a guarantee that material already received by another service is erased.

## Review and recovery

Workspace revisions and change history help you inspect edits and undo supported changes. Review comments provide a record of discussion. History is not a substitute for backups, and recovery depends on the instance's retention configuration. For concurrent changes, inspect conflicts rather than forcing an overwrite.

## Operating requirements and costs

An Atlas instance needs its configured cloud services, authentication and an operator. The standard deployment uses Azure. The actual cost depends on the installed apps, activity, storage, hosting, retention and inference choices.

Workspace and KB do not need a separate semantic database or embedding provider for ordinary editing and navigation. Search is an optional advanced capability that needs a database and compatible inference for semantic retrieval, plus indexing resources. AI subscriptions and provider usage are separate from Atlas hosting. No fixed monthly cost or service-level commitment is implied.

See [Deployment]({{ site.baseurl }}{% link docs/developers/deployment.md %}) for prerequisites and [KB versus Search]({{ site.baseurl }}{% link docs/articles/kb-versus-search.md %}) for the trade-off. Confirm app availability, licensing and operating responsibility with the instance owner.

The security specifications describe intended and implemented controls; this guide is not an independent security certification or a guarantee that every deployment has passed enterprise acceptance testing.
