# Node Lint action

Install dependencies, optionally cache Next.js builds, and run one or more lint
scripts.

## Usage

```yaml
name: Lint

on: [push]

jobs:
    lint:
        runs-on: ubuntu-latest
        steps:
            - uses: krudi/reusable-actions/.github/actions/node-build-lint@main
              with:
                  node_version: '22.18.0'
                  nextjs: 'false'
                  lint_commands: |
                      lint
                      lint:css
```

## Inputs

| Input           | Default   | Description                                                                    |
| --------------- | --------- | ------------------------------------------------------------------------------ |
| `node_version`  | `22.18.0` | Node.js version to install.                                                    |
| `nextjs`        | `false`   | `true` to cache `.next/cache` and npm.                                         |
| `lint_commands` | `""`      | Newline-separated list of `package.json` scripts; missing scripts are skipped. |
