---
title: Development environment
parent: Developers
nav_order: 1
---

# Set up a development environment

**For Hobby or Work, clone only the edition you want to develop.** You can build, test and deploy that edition using released Core packages. The edition does not need a Core source checkout or a synchronised Core branch. Your Git credentials must be able to read Core’s release tags and packages.

Install Git, Python 3.12 or newer, Node.js with npm, and PowerShell. Core’s validation workflow uses Python 3.13 and Node.js 24. Use PowerShell 7.2 or later if you use the managed settings store. Azure tooling is needed for [deployment]({{ site.baseurl }}{% link docs/developers/deployment.md %}).

## Core

To change Core itself, open PowerShell in its checkout and install its Python packages and web dependencies.

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
./devops/dev-install.ps1
npm ci
```

Run Python tests from the backend or package directory with `python -m pytest`. For web changes, run the affected app’s `typecheck` or `build` script as documented in its `AGENTS.md`. Vitest alone does not typecheck the app.

## Hobby or Work

In the edition checkout, activate the Python environment you want to use and run the edition’s installer. It uses the existing environment, installs the edition’s Python projects in editable mode, runs `npm ci` and resolves the Core packages it needs.

```powershell
./devops/dev-install.ps1
```

If Core packages are absent, `atlas-core-resolve` uses the newest published release. Otherwise it uses the installed packages, subject to the minimum in `testing/suites.json` and the latest major version. Use `./devops/dev-install.ps1 -RefreshCore` when you want to refresh a release-based setup. Set `ATLAS_CORE_PATH` only if you deliberately want to develop against a Core checkout, for example to test a shared-package change before release.

For an edition, `atlas-test --manifest testing/suites.json --runner pytest` or `--runner npm` runs the manifest’s suites. If you run `npm ci` manually in an edition, follow it with `atlas-core-resolve ensure --project . --npm-only` to restore Core’s unsaved npm packages.

## After pulling changes

Run `dev-install.ps1` once for a checkout and Python environment, then use `git pull` for ordinary source updates. Editable Python installs read the updated source directly. Rerun the installer when a change adds a module or Python package, adds or changes a CLI entry point, alters dependency metadata, or when you switch to a fresh Python environment. Refresh npm dependencies when the lockfile changes. An edition pull does not refresh the installed Core release.

## Instance configuration

If your development task needs instance settings, use the project’s `.env.example` files as the configuration reference and set `ATLAS_INSTANCE` to select the instance. With `ATLAS_ENV_PROVIDER` configured, `dev-install.ps1` also materialises the settings. For a settings-only change, use the provider directly instead of reinstalling dependencies. See [deployment settings]({{ site.baseurl }}{% link docs/developers/deployment.md %}#prepare-deployment-settings).
