# PHP Build action

Set up PHP, cache Composer dependencies, and install.

## Usage

```yaml
name: Build (PHP)

on: [push]

jobs:
    build:
        runs-on: ubuntu-latest
        steps:
            - uses: krudi/reusable-actions/.github/actions/php-build@main
              with:
                  php_version: '8.3'
                  extensions: 'mbstring, dom, zip'
                  tools: 'composer:v2.5'
```

## Inputs

| Input             | Default                                          | Description                                    |
| ----------------- | ------------------------------------------------ | ---------------------------------------------- |
| `php_version`     | `8.3`                                            | PHP version to install.                        |
| `extensions`      | `mbstring, dom, zip`                             | Comma-separated extensions to enable.          |
| `tools`           | `composer:v2.5`                                  | Tools to install (via `shivammathur/setup-php`). |
| `install_command` | `composer install --no-progress --ignore-platform-reqs` | Command used to install dependencies. |
