# Node Build action

Install dependencies, optionally cache Next.js builds, and run `npm run build`.

## Usage

```yaml
name: Build

on: [push]

jobs:
    build:
        runs-on: ubuntu-latest
        steps:
            - uses: krudi/reusable-actions/.github/actions/node-build
              with:
                  node_version: '22.18.0'
                  nextjs: 'false'
```

## Inputs

| Input          | Default   | Description                            |
| -------------- | --------- | -------------------------------------- |
| `node_version` | `22.18.0` | Node.js version to install.            |
| `nextjs`       | `false`   | `true` to cache `.next/cache` and npm. |
