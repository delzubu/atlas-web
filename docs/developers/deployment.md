---
title: Deployment
parent: Developers
nav_order: 3
---

# Deployment

Deploy Core first to create the Atlas instance. It provides Identity, the launcher, and shared services. Hobby and Work deploy their own Azure Function Apps and static sites into that instance. Run each deployment from its project checkout. An edition deployment needs an existing instance and released Core packages. You can deploy it without checking out or synchronising Core source code.

## Install the deployment tools

Use Git, PowerShell, Python, Node.js and npm as described in [Development environment]({{ site.baseurl }}{% link docs/developers/setup.md %}). Install Azure CLI and Azure Functions Core Tools v4, then sign in to Azure with `az login`. For first-time Core setup, the deployer needs permission to provision resources and create role assignments, such as Owner or Contributor plus User Access Administrator. An edition deployer needs the relevant permissions on the instance and edition resource groups.

## Prepare deployment settings

Every deployment needs the selected instance’s settings. **`atlas-secrets` is required when you deploy an instance managed by the Atlas encrypted store.** A self-managed instance can use another provider or untracked `.env.<instance>` files maintained from the project’s example templates. The deploy scripts call a provider only when `ATLAS_ENV_PROVIDER` is set.

### Managed settings with atlas-secrets

1. Get authorised access to the private `atlas-secrets` repository and clone it. Install PowerShell 7.2 or later, SOPS 3.11 or later, and age. On Windows the Winget package IDs are `Microsoft.PowerShell`, `Mozilla.SOPS`, and `FiloSottile.age`.
2. Follow that repository’s README to provision the authorised age identity for this machine. Keep the private key outside the checkout. SOPS can read it from its standard key location or the file named by `SOPS_AGE_KEY_FILE`.
3. Ensure the store contains settings for the selected instance and project. For an existing hand-managed instance, follow the store’s import guide once and review the result before switching to generated files.
4. From the secrets checkout, set the two user environment variables below. Replace `<instance>` with the instance you intend to use. Open a fresh PowerShell session afterwards so the variables are available to Atlas.

```powershell
[Environment]::SetEnvironmentVariable('ATLAS_ENV_PROVIDER', (Resolve-Path './scripts/atlas-env.ps1').Path, 'User')
[Environment]::SetEnvironmentVariable('ATLAS_INSTANCE', '<instance>', 'User')
```

Use `ATLAS_INSTANCE` for new setups. `ATLAS_PROFILE` is a legacy alias supported by the backend settings loader. Pass `-Instance` explicitly to validate or deploy a different instance.

The provider writes the root and app-level `.env.<instance>` files before validation or deployment. To refresh settings independently, run the provider from the project checkout with `materialize -Instance <instance> -Project .`. Change a generated setting in the encrypted store, then materialise it again. After a deploy, the provider records newly discovered values back into the store. Review and commit the encrypted changes there.

## Validate the selected project

From the Core or edition checkout, replace `<instance>` with your instance name.

```powershell
./devops/deploy.ps1 -Instance <instance> -ValidateOnly
```

Validation checks the configuration without Azure calls. With a provider configured, it may refresh local settings files. Resolve missing settings or provider errors before proceeding.

## Deploy Core for the first time

Follow `INSTALL.md` and `devops/DEPLOY.md` in the Core repository for the full sequence. Prepare the Azure subscription and the Google or Microsoft Entra ID registration for your instance. Deploy infrastructure and the backend, configure the sign-in redirect URIs and bootstrap the required stores. Deploy the website, register OAuth clients and create the initial authorised user. Seed the Core library and verify sign-in and app access.

## Upgrade Core

Follow the Core deployment guide for the upgrade. Run `atlas-admin bootstrap` again when a release adds Identity storage containers, as Core 1.1.2 does for temporary Search connections. Bootstrap is safe to rerun. Identity can also create a missing known container on first use. Seed the updated Core library to make the release’s playbooks available.

For Search in Core 1.2.0, configure the shared inference service and available models in Identity under **Administration → Settings**. Move inference settings from existing database connections before saving those connections in the new UI. If your deployment uses doc-indexer for reranking, import its updated `ui_rerank` workflow to support per-database model selection. Run `search-intake worker` on an authorised computer to process queued indexing jobs. [Core upgrade and inference details](https://github.com/delzubu/atlas-core/pull/36)

## Add Hobby or Work

1. Have an instance administrator create or retrieve the instance deploy key in Identity under **Administration → Modules → Deploy key**.
2. In your edition checkout, complete [edition setup]({{ site.baseurl }}{% link docs/developers/setup.md %}#hobby-or-work). Supply the existing instance resource group, the edition resource group and deploy key using the project’s settings contract.
3. Validate, then follow the edition’s `devops/DEPLOY.md` to run `./devops/deploy.ps1 -Instance <instance>` as the deployment owner. The edition discovers the instance’s addresses and registers its services and clients with Identity.
4. Grant the users access to the new apps. Follow the edition’s library-seeding instructions to make its playbooks and templates available, then verify the apps through the launcher.

Updates to one edition deploy from that edition. A Core release does not trigger an edition rebuild or deploy. Refresh the edition’s Core packages deliberately when you need a newer release.
