# npm Publish

Publish npm packages with optional workspace support and provenance.

## Usage

Publish a workspace derived from the tag (`@scope/pkg@1.2.3`):

```yaml
- uses: krudi/reusable-actions/.github/actions/npm-publish@main
  with:
    tag: ${{ github.ref_name }}
  env:
    NODE_AUTH_TOKEN: ${{ secrets.NPM_TOKEN }}
```

Publish a specific workspace:

```yaml
- uses: krudi/reusable-actions/.github/actions/npm-publish@main
  with:
    workspace: packages/sitepackage
    access: public
    provenance: true
  env:
    NODE_AUTH_TOKEN: ${{ secrets.NPM_TOKEN }}
```

Publish the root package (no workspace):

```yaml
- uses: krudi/reusable-actions/.github/actions/npm-publish@main
  with:
    access: public
    provenance: true
  env:
    NODE_AUTH_TOKEN: ${{ secrets.NPM_TOKEN }}
```

## Inputs

- `workspace` (optional): Workspace name to publish. Leave blank to publish the root.
- `tag` (optional): Tag like `@scope/pkg@1.2.3`; used to derive the workspace when not provided (falls back to `github.ref_name`).
- `access` (default `public`): Access flag for npm publish.
- `provenance` (default `true`): Whether to pass `--provenance`.
- `npm_token` (optional): NPM token; falls back to `NODE_AUTH_TOKEN` env.
- `extra_args` (optional): Extra flags passed to `npm publish`.
