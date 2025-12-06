# CI Command

Run any CI command provided as input (good for matrix lint/test steps).

```yaml
- uses: krudi/reusable-actions/.github/actions/ci-command@main
  with:
      command: npm test
```

Matrix example:

```yaml
strategy:
    fail-fast: false
    matrix:
        lint-commands:
            - 'npm run lint:eslint'
            - 'npm run lint:stylelint'

steps:
    - uses: krudi/reusable-actions/.github/actions/ci-command@main
      with:
          command: ${{ matrix.lint-commands }}
```

This stays single-line (matrix-friendly) so you don’t need a multiline `run: |` block.

Outputs:

- `command_run`: the command executed
- `exit_code`: exit code of the command
- `duration_ms`: how long the command took (ms)
