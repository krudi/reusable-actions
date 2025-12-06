# Reusable GitHub Actions

Composite actions for common Node.js steps. Keep workflows lean by reusing these blocks instead of duplicating setup
logic across repos.

## Actions at a glance

| Action     | Path                              | What it does                                                                      |
| ---------- | --------------------------------- | --------------------------------------------------------------------------------- |
| Node Build | `.github/actions/node-build`      | Install dependencies, optional Next.js cache, run `npm run build`.                |
| Node Lint  | `.github/actions/node-build-lint` | Install dependencies, optional Next.js cache, run newline-separated lint scripts. |
| Typecheck  | `.github/actions/node-typecheck`  | Install dependencies, run a single typecheck script.                              |

## Quick start

1. Create a workflow in your project (e.g. `.github/workflows/ci.yaml`).
2. Use the action you need: `uses: krudi/reusable-actions/.github/actions/<action>@main`.

Usage examples (with inputs) live alongside each action:

- [`node-build`](.github/actions/node-build/README.md)
- [`node-build-lint`](.github/actions/node-build-lint/README.md)
- [`node-typecheck`](.github/actions/node-typecheck/README.md)

## Inputs

### Node Build

| Input          | Default   | Description                            |
| -------------- | --------- | -------------------------------------- |
| `node_version` | `22.18.0` | Node.js version to install.            |
| `nextjs`       | `false`   | `true` to cache `.next/cache` and npm. |

### Node Lint

| Input           | Default   | Description                                                                    |
| --------------- | --------- | ------------------------------------------------------------------------------ |
| `node_version`  | `22.18.0` | Node.js version to install.                                                    |
| `nextjs`        | `false`   | `true` to cache `.next/cache` and npm.                                         |
| `lint_commands` | `""`      | Newline-separated list of `package.json` scripts; missing scripts are skipped. |

### Typecheck

| Input               | Default   | Description                                                |
| ------------------- | --------- | ---------------------------------------------------------- |
| `node_version`      | `22.18.0` | Node.js version to install.                                |
| `typecheck_command` | `""`      | `package.json` script to run; skipped if empty or missing. |
