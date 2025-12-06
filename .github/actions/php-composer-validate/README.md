# PHP Composer Validate

Composite action that runs `composer validate` (or a custom command) in a given working directory.

## Inputs

- `working_directory` (default `.`)
- `validate_command` (default `composer validate`)

## Example

```yaml
- uses: krudi/reusable-actions/.github/actions/php-composer-validate@main
  with:
    working_directory: app
```
