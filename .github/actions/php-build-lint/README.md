# PHP Build Lint action

Set up PHP, cache Composer dependencies, install, and run newline-separated lint commands.

## Usage

```yaml
name: Lint (PHP)

on: [push]

jobs:
    lint:
        runs-on: ubuntu-latest
        steps:
            - uses: krudi/reusable-actions/.github/actions/php-build-lint@main
              with:
                  php_version: '8.3'
                  extensions: 'mbstring, dom, zip'
                  tools: 'composer:v2.5'
                  lint_commands: |
                      - "composer lint:php-cs-fixer"
                      - "composer lint:phpstan"
```

## Inputs

| Input             | Default                                          | Description                                           |
| ----------------- | ------------------------------------------------ | ----------------------------------------------------- |
| `php_version`     | `8.3`                                            | PHP version to install.                               |
| `extensions`      | `mbstring, dom, zip`                             | Comma-separated extensions to enable.                 |
| `tools`           | `composer:v2.5`                                  | Tools to install (via `shivammathur/setup-php`).      |
| `install_command` | `composer install --no-progress --ignore-platform-reqs` | Command used to install dependencies.        |
| `lint_commands`   | `""`                                             | Newline-separated commands to run; skipped if empty.  |
