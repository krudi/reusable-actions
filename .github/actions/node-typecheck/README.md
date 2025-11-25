# Typecheck action

Install dependencies and run a TypeScript typecheck (or any single script you
specify).

## Usage

```yaml
name: Typecheck

on: [push]

jobs:
    typecheck:
        runs-on: ubuntu-latest
        steps:
            - uses: krudi/reusable-actions/.github/actions/node-typecheck@main
              with:
                  node_version: '22.18.0'
                  typecheck_command: 'tsc'
```

## Inputs

| Input               | Default   | Description                                                |
| ------------------- | --------- | ---------------------------------------------------------- |
| `node_version`      | `22.18.0` | Node.js version to install.                                |
| `typecheck_command` | `""`      | `package.json` script to run; skipped if empty or missing. |
