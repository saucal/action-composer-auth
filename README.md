# action-composer-auth

Configure composer HTTP basic auth for a private composer repository.

Generic wrapper around `composer config http-basic.<domain> <username> <password>` — the single
source of truth for composer authentication across Saucal actions. SatisPress
(`packages.saucal.com`) is the default, but it works for any private composer repo.

## Usage

SatisPress (defaults cover it — just pass the key):

```yaml
- uses: saucal/action-composer-auth@v1
  with:
    username: ${{ secrets.SAUCAL_SATIS_KEY || secrets.SAUCAL_GLOBAL_SATIS_KEY }}
    path: source        # dir containing composer.json (default: .)
```

Any other private repo (pass domain + explicit credentials):

```yaml
- uses: saucal/action-composer-auth@v1
  with:
    domain: composer.example.com
    username: ${{ secrets.EXAMPLE_USER }}
    password: ${{ secrets.EXAMPLE_TOKEN }}
```

Authenticate multiple repos by calling the action once per repo.

## Inputs

| Input | Default | Description |
|-------|---------|-------------|
| `domain` | *(empty)* | Repository domain — the composer http-basic key. Empty ⇒ resolves from the `PACKAGE_SERVER` repo/org variable, then `packages.saucal.com`. |
| `username` | `''` | Auth username (SatisPress key). Empty ⇒ no-op. |
| `password` | `''` | Auth password. Empty ⇒ defaults to the project's `homepage` (protocol stripped), the SatisPress convention. Pass explicitly for other repos. |
| `path` | `.` | Project dir containing `composer.json`. auth.json is written here. |

## Notes

Auth is written locally in `path`, so a composer run in that directory is authenticated.
Composer must be available on the runner. No-op if `path` has no `composer.json`.
