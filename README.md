# action-composer-auth

Configure composer HTTP basic auth for a SatisPress instance (`packages.saucal.com`).

This is the **single source of truth** for composer ⇄ SatisPress credentials across Saucal
actions. Any action or workflow that runs composer against private packages should call this
instead of duplicating the `composer config http-basic …` line.

## Usage

```yaml
- uses: saucal/action-composer-auth@v1
  with:
    satis_key: ${{ secrets.SAUCAL_SATIS_KEY || secrets.SAUCAL_GLOBAL_SATIS_KEY }}
    path: source          # dir containing composer.json (default: .)
```

## Inputs

| Input | Default | Description |
|-------|---------|-------------|
| `satis_key` | `''` | SatisPress key. Empty ⇒ no-op. |
| `path` | `.` | Project dir containing `composer.json`. auth.json is written here; the password is derived from the project's `homepage`. |
| `host` | `packages.saucal.com` | SatisPress host. |

## Convention

`username` = the SatisPress key, `password` = the project's `homepage` (protocol stripped).
Auth is written locally in `path` (`composer config http-basic.<host> …`), so a composer run
in that directory is authenticated. Composer must be available on the runner.

Requires the `SAUCAL_GLOBAL_SATIS_KEY` org secret to be visible to the consuming repo.
