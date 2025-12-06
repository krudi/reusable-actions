# Node Setup

Set up a Node.js environment with optional cache and version-file support.

```yaml
- uses: krudi/reusable-actions/.github/actions/node-setup@main
  with:
    cache: npm                 # npm|yarn|pnpm|"" (default: npm)
    node_version: "22.18.0"    # optional, ignored if node_version_file is set
    node_version_file: .nvmrc  # optional, e.g. .nvmrc or .node-version
```

Inputs:
- `node_version` (default `22.18.0`): Node version to install when no version file is provided.
- `node_version_file` (default empty): Path to a version file such as `.nvmrc` or `.node-version`.
- `cache` (default `npm`): Package manager cache to enable (`npm`, `yarn`, `pnpm`, or empty to disable).
