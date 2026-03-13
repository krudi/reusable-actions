# reusable-workflows

Reusable GitHub Actions building blocks for my projects. This repo now contains both composite actions and reusable
workflows so release, publish, and deploy logic can stay consistent across repositories.

> [!WARNING]
>
> These are built for my personal projects and may not fit every setup without adjustment.

## Composite actions

| Action             | Path                                 | What it does                                              |
| ------------------ | ------------------------------------ | --------------------------------------------------------- |
| GitHub Checkout    | `.github/actions/github-checkout`    | Checkout repository at current ref.                       |
| CI Command         | `.github/actions/ci-command`         | Run a provided CI command (matrix-friendly).              |
| Node Setup         | `.github/actions/node-setup`         | Set up Node with optional cache and version-file support. |
| Node Install       | `.github/actions/node-install`       | Install Node dependencies (no checkout/setup).            |
| Node Build         | `.github/actions/node-build`         | Build Node projects (no checkout/setup).                  |
| Node Next Cache    | `.github/actions/node-next-cache`    | Cache npm and `.next/cache`.                              |
| npm Publish        | `.github/actions/npm-publish`        | Publish npm packages (workspace or root) with provenance. |
| PHP Setup          | `.github/actions/php-setup`          | Set up PHP with extensions/tools.                         |
| PHP Composer Cache | `.github/actions/php-composer-cache` | Cache Composer deps and vendor.                           |
| PHP Install Run    | `.github/actions/php-install-run`    | Install PHP dependencies (no checkout/setup).             |

## Reusable workflows

| Workflow            | Path                               | What it does                                                   |
| ------------------- | ---------------------------------- | -------------------------------------------------------------- |
| Create Release      | `.github/workflows/create-release` | Bump version, commit, tag, and create a GitHub release.        |
| Publish NPM Package | `.github/workflows/publish-npm`    | Install, optionally check/build, and publish a package to npm. |
| Publish VS Code     | `.github/workflows/publish-vscode` | Install, check, package, upload VSIX, and publish extension.   |
| Deploy TYPO3        | `.github/workflows/deploy-typo3`   | Run the shared TYPO3 deploy pipeline.                          |

## Quick start

1. Create a workflow in your project (e.g. `.github/workflows/ci.yaml`).
2. Use the action or workflow you need.
3. Prefer immutable refs for callers.

Recommended:

```yaml
uses: krudi/reusable-actions/.github/workflows/publish-npm.yaml@v1
```

Temporary during active development:

```yaml
uses: krudi/reusable-actions/.github/workflows/publish-npm.yaml@main
```

Use `@main` only while iterating. Once the reusable interface is stable, move callers to a tagged ref such as `@v1`.

Usage examples (with inputs) live alongside each action:

- [`ci-command`](.github/actions/ci-command/README.md)
- [`github-checkout`](.github/actions/github-checkout/README.md)
- [`node-setup`](.github/actions/node-setup/README.md)
- [`node-install`](.github/actions/node-install/README.md)
- [`node-build`](.github/actions/node-build/README.md)
- [`node-next-cache`](.github/actions/node-next-cache/README.md)
- [`php-setup`](.github/actions/php-setup/README.md)
- [`php-composer-cache`](.github/actions/php-composer-cache/README.md)
- [`php-install-run`](.github/actions/php-install-run/README.md)

## Workflow contracts

### `create-release`

Use this for manual `workflow_dispatch` release entrypoints. It:

- validates the requested version
- bumps the target package version
- verifies the expected file version
- fails if the target tag already exists
- fails if unexpected files change during the version bump
- commits only the allowed release files
- pushes the branch, creates the tag, and creates the GitHub release

Important inputs:

- `package_json_path`
- `package_lock_path`
- `workspace`
- `tag_prefix`
- `commit_message`
- `extra_commit_paths`

### `publish-npm`

Use this for `release.published` npm publishing jobs.

Important inputs:

- `workspace`
- `check_command`
- `build_command`
- `npm_access`
- `npm_provenance`

### `publish-vscode`

Use this for `release.published` VS Code extension publishing jobs.

Important inputs:

- `check_command`
- `package_command`
- `publish_command`
- `vsce_glob`

### `deploy-typo3`

Use this for `release.published` or manual deploy wrappers in TYPO3 repos.

Important inputs:

- `php_version`
- `composer_install_command`
- `node_install_command`
- `node_build_command`
- `dep_command`

## Release tag conventions

Current release contracts in caller repos:

- single-package repos: `vX.Y.Z`
- `shared-configs`: `@krudi/<package>@X.Y.Z`
- `shared-styles`: `@krudi/<package>@X.Y.Z`
- TYPO3 repos: `package.json` is the release version source of truth; `composer.json` is not versioned

Examples:

- `@krudi/eslint-config@0.1.9`
- `@krudi/styles@0.1.3`
- `v0.6.0`

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

Monorepo package release + publish

```yaml
name: Release eslint-config

on:
    workflow_dispatch:
        inputs:
            version:
                required: true
                type: string

jobs:
    release:
        uses: krudi/reusable-actions/.github/workflows/create-release.yaml@v1
        with:
            version: ${{ inputs.version }}
            workspace: '@krudi/eslint-config'
            package_json_path: packages/eslint-config/package.json
            package_lock_path: package-lock.json
            tag_prefix: '@krudi/eslint-config@'
            commit_message: 'chore(release): bump @krudi/eslint-config from {old} to {new}'
```
