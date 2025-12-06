# reusable-workflows

Composite actions for common Node.js and PHP steps. Keep workflows lean by reusing these blocks instead of duplicating
setup logic across repos.

> [!WARNING]
>
> These are built for my personal projects and may not fit every setup without adjustment.

## Quick overview of actions

| Action             | Path                                    | What it does                                              |
| ------------------ | --------------------------------------- | --------------------------------------------------------- |
| GitHub Checkout    | `.github/actions/github-checkout`       | Checkout repository at current ref.                       |
| CI Command         | `.github/actions/ci-command`            | Run a provided CI command (matrix-friendly).              |
| Node Setup         | `.github/actions/node-setup`            | Set up Node with optional cache and version-file support. |
| Node Install       | `.github/actions/node-install`          | Install Node dependencies (no checkout/setup).            |
| Node Build         | `.github/actions/node-build`            | Build Node projects (no checkout/setup).                  |
| Node Next Cache    | `.github/actions/node-next-cache`       | Cache npm and `.next/cache`.                              |
| npm Workspace Pub  | `.github/actions/npm-workspace-publish` | Publish npm workspace packages with provenance.           |
| PHP Setup          | `.github/actions/php-setup`             | Set up PHP with extensions/tools.                         |
| PHP Composer Cache | `.github/actions/php-composer-cache`    | Cache Composer deps and vendor.                           |
| PHP Install Run    | `.github/actions/php-install-run`       | Install PHP dependencies (no checkout/setup).             |
| PHP Deploy         | `.github/actions/php-deploy`            | Run Deployer-based deployments.                           |

## Quick start

1. Create a workflow in your project (e.g. `.github/workflows/ci.yaml`).
2. Use the action you need: `uses: krudi/reusable-actions/.github/actions/<action>@main`.

Usage examples (with inputs) live alongside each action:

- [`ci-command`](.github/actions/ci-command/README.md)
- [`github-checkout`](.github/actions/github-checkout/README.md)
- [`node-setup`](.github/actions/node-setup/README.md)
- [`node-install`](.github/actions/node-install/README.md)
- [`node-build`](.github/actions/node-build/README.md)
- [`node-next-cache`](.github/actions/node-next-cache/README.md)
- [`npm-workspace-publish`](.github/actions/npm-workspace-publish/README.md)
- [`php-setup`](.github/actions/php-setup/README.md)
- [`php-composer-cache`](.github/actions/php-composer-cache/README.md)
- [`php-install-run`](.github/actions/php-install-run/README.md)
- [`php-deploy`](.github/actions/php-deploy/README.md)

## Real-world workflow examples

Node lint matrix

```yaml
name: Lint

on:
    pull_request:
        types: [opened, synchronize, reopened]

jobs:
    lint:
        runs-on: ubuntu-latest
        strategy:
            fail-fast: false
            matrix:
                lint-commands:
                    - 'npm run lint:eslint'
                    - 'npm run lint:stylelint'

        steps:
            - uses: krudi/reusable-actions/.github/actions/github-checkout@main

            - uses: krudi/reusable-actions/.github/actions/node-setup@main
              with:
                  cache: npm
                  node_version_file: .nvmrc

            - uses: krudi/reusable-actions/.github/actions/node-install@main

            - uses: krudi/reusable-actions/.github/actions/ci-command@main
              with:
                  command: ${{ matrix.lint-commands }}
```
