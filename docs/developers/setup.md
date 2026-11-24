---
title: Development environment
parent: Developers
nav_order: 1
---

# Set up a development environment

Use Git, Python 3.12 or newer, Node.js and npm, and PowerShell. Check the repository's build configuration for its Node.js version. Azure CLI and Azure Functions Core Tools are part of the [deployment setup]({{ site.baseurl }}{% link docs/developers/deployment.md %}).

## Core

Open a PowerShell session in the Core checkout and install the Python packages and web dependencies.

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
./devops/dev-install.ps1
npm ci
```

Run Python tests from the backend or package directory with `python -m pytest`. For web changes, use the affected app's typecheck or build command.

## An edition

Open PowerShell in the edition checkout with your Python environment already active. Run `./devops/dev-install.ps1` to resolve Core and install the edition's Python and npm dependencies. If you later run `npm ci` in the edition, restore the Core npm packages with `atlas-core-resolve ensure --project . --npm-only`.

## Instance configuration

If your development task needs instance settings, use the project's `.env.example` files as the configuration reference and set `ATLAS_INSTANCE` to select the instance. If your team uses an environment provider, follow its setup instructions to materialise the local settings. Keep credentials out of source control.
