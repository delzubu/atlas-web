---
title: Getting started
nav_order: 2
has_children: true
has_toc: false
---

# Getting started

Atlas runs as an instance: a deployment with its own users, enabled apps, storage and access policies. You use its browser interface alongside the AI agent or agents you choose.

## Choose your starting point

| Situation | Next step |
| --- | --- |
| You have been invited to an instance | Obtain its launcher address and sign-in instructions, then follow the steps below. |
| You are evaluating Atlas | Read the [first shared project walkthrough]({{ site.baseurl }}{% link docs/getting-started/first-project.md %}) and explore the [apps]({{ site.baseurl }}{% link docs/apps/index.md %}). The walkthrough includes example artifacts and can be read without signing in. |
| You want to operate an instance | Read [Deployment]({{ site.baseurl }}{% link docs/developers/deployment.md %}) and the [operating requirements]({{ site.baseurl }}{% link docs/getting-started/data-and-access.md %}#operating-requirements-and-costs). |

This website documents Atlas; it does not provision accounts or offer a public sandbox. Access to an existing instance is arranged with its administrator. Self-managed deployment requires the appropriate source access, licence, cloud resources and operational permissions. No public hosted trial or subscription pricing is established by this guide.

## Open Atlas in your browser

1. Open the launcher and choose the sign-in option configured for your instance.
2. Open Workspace. If an app is missing, ask your administrator whether it is installed and whether your account has access.
3. Select an existing project or create a small project with writer access. You can start empty or from a template.

## Choose and connect an agent

Atlas is model-agnostic, not a model subscription. Use a compatible client with the appropriate MCP connection and Atlas permissions.

| Client route | How to connect |
| --- | --- |
| ChatGPT | Use the Atlas plugin or registered connections supplied for your instance. Sign in to Atlas when prompted. |
| Claude Code | Use the instance's Claude-compatible plugin or MCP configuration. |
| Other MCP clients | Obtain the instance's Streamable HTTP connection and authentication details; verify the client's OAuth and tool support. |
| Atlas Assistant | Open the panel if configured; use the hosted agent and toolkits available in your instance. |

The [plugin documentation](https://github.com/delzubu/atlas-core/tree/main/plugins) describes the ChatGPT and Claude Code packaging routes for operators. Client and subscription support varies; a plugin or endpoint definition does not guarantee that a particular client can perform write actions. Ask the administrator for the configuration appropriate to your deployment, not a connection belonging to a different instance.

Start with a read-only request: ask the agent to list the projects it can access and read a known file. Then try a small edit in a project where you have write access. Do not give the agent storage-account credentials.

## Your first useful result

Follow [Create and review your first shared artifact]({{ site.baseurl }}{% link docs/getting-started/first-project.md %}). You will create a short brief, review a comment and ask an agent to revise the same file. No separate Search database is needed.

Once you have a working connection, you can switch to another compatible agent and continue on the same project. Chat histories and provider usage limits remain separate.

## Continue exploring

Read [Your data and agent access]({{ site.baseurl }}{% link docs/getting-started/data-and-access.md %}) to understand storage, permissions and operating requirements. Browse the [app directory]({{ site.baseurl }}{% link docs/apps/index.md %}), [playbooks and templates]({{ site.baseurl }}{% link docs/playbooks/index.md %}) and [articles]({{ site.baseurl }}{% link docs/articles/index.md %}) for further uses.
