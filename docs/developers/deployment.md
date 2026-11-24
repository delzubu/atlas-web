---
title: Deployment
parent: Developers
nav_order: 3
---

# Deployment

An Atlas instance starts with Core. Core deploys its backend services to an Azure Function App and publishes its web apps as static files. Each edition deploys its own Function App and web apps, then registers them with the instance.

## Prepare an instance

Follow `INSTALL.md` in the Core repository for Azure prerequisites, Google or Microsoft Entra ID sign-in, instance settings, and first-time setup. Install Azure CLI and Azure Functions Core Tools v4 on the machine you deploy from.

Before deploying, check the configuration from the Core checkout. Replace `<instance>` with your instance name.

```powershell
./devops/deploy.ps1 -Instance <instance> -ValidateOnly
```

## Add an edition

Follow `devops/DEPLOY.md` in the edition repository. The edition deploys separately and registers its services and clients with Core. After registration, grant users access to the new apps through Identity.
