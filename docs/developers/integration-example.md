---
title: First integration
parent: Developers
nav_order: 6
---

# Your first Atlas integration

Use an existing compatible MCP client to exercise the shared Workspace contract before attempting a new application. This walkthrough requires an authorised instance and a disposable workspace. It describes the operations to request rather than assuming a particular client SDK or inventing tool signatures.

## Connect and discover

Obtain the deployment-specific Workspace MCP endpoint and OAuth configuration from the operator. Use the supported plugin route or configure the client with Streamable HTTP and the advertised OAuth discovery information. Sign in as the intended user. Consult the [plugin documentation](https://github.com/delzubu/atlas-core/tree/main/plugins) for packaging and platform details.

Discover tools from the connected server. Inspect their schemas and annotations; do not assume the names and parameter shapes of an older MCP version. Ask the client to list visible workspaces and open a test workspace. Verify that the intended project and ref are selected.

## Read and modify one artifact

Create a disposable file in the test workspace through the browser, for example `integration-check.md` containing a heading and one sentence.

Ask the connected agent to perform this bounded task:

> Open the named test workspace and read integration-check.md with its revision information. Append a short section called Integration check. Preserve the existing content. Use the current revision or other concurrency precondition required by the discovered write tool. Return the resulting Atlas reference and a description of the change.

Open the file in the browser and inspect history. The result should be persisted and attributed to the authorised actor. Copy the Atlas reference; use its resolved HTTPS form only where the receiving client cannot resolve Atlas URIs.

## Check failure paths

- With a read-only identity or grant, repeat the read and attempt a write to the disposable file. The read should work; the write should be denied.
- Read the file, then change it from another authorised session. Attempt a conflicting write using the earlier precondition. Handle the conflict by rereading and reconciling changes; do not silently overwrite.
- Verify that an inaccessible workspace is not exposed through discovery or direct lookup.
- Inspect the resulting history and undo the test edit where supported.

The exact status and tool error shapes are defined by the [authorization contract](https://github.com/delzubu/atlas-core/blob/main/docs/specs/authorization.md) and [workspace concurrency contract](https://github.com/delzubu/atlas-core/blob/main/docs/specs/workspace-concurrency.md). These checks are an exercise, not evidence that a particular deployment has passed them.

## Command-line alternative

For a user-authenticated CLI integration, install the Workspace client following the source [installation instructions](https://github.com/delzubu/atlas-core/blob/main/INSTALL.md). Set `ATLAS_URL` to the instance's suite endpoint and run:

```powershell
ws-tool login
ws-tool whoami
ws-tool list-workspaces
ws-tool --project integration-test --ref main read integration-check.md
```

Replace the project name and ref with the disposable project you created. The normal client uses Atlas sign-in, not a storage-account key. See the [Workspace CLI reference](https://github.com/delzubu/atlas-core/blob/main/apps/workspace/api/README.md) for write operations and concurrency behaviour.

## Where to go next

For a new application, use the shared packages described in [SDK and integrations]({{ site.baseurl }}{% link docs/developers/sdk.md %}). Keep app entitlement checks, Workspace grants, revision preconditions and shared design rules in the implementation. Review the [design system]({{ site.baseurl }}{% link docs/developers/design/index.md %}) and source contracts before implementing your own screens or document operations.
