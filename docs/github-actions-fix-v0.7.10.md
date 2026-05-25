# GitHub Actions CI fix for v0.7.10

This patch updates `.github/workflows/phpunit.yml` so the WordPress PHPUnit jobs install the required system dependencies before running the WordPress test-suite installer.

## Fixed CI failure

Previous failure:

```text
scripts/install-wp-tests.sh: line 32: svn: command not found
```

Root cause: the Ubuntu GitHub runner did not have Subversion installed, but WordPress' test-suite installer uses `svn` to download PHPUnit includes/data.

## Fix added

```yaml
- name: Install system packages
  run: |
    sudo apt-get update
    sudo apt-get install -y subversion default-mysql-client
```

The workflow also enables required PHP extensions:

```yaml
extensions: dom, xml, mbstring, mysqli, curl, zip, json, openssl, sodium
```

## How to use

Commit this workflow file to:

```text
.github/workflows/phpunit.yml
```

Then rerun **Actions → SafeDate PHPUnit**.
