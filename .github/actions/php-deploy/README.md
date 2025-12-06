# PHP Deploy

Composite action that runs a Deployer command with provided SSH config/keys. It assumes you have already checked out the
repo and prepared dependencies/build artifacts.

## Inputs

- `dep_command` (default `dep deploy production`)
- `known_hosts` (**required**)
- `private_key` (**required**)

## Example

```yaml
- uses: krudi/reusable-actions/.github/actions/php-deploy@main
  with:
      known_hosts: ${{ secrets.SSH_SERVER_HOSTS }}
      private_key: ${{ secrets.SSH_PRIVATE_KEY }}
```
