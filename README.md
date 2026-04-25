# kenopahq releases

Public distribution hub for kenopahq's macOS applications. Source code lives in
private repos; this repo hosts the public release artifacts (DMG, zip) and the
Sparkle appcast XML files used for in-app auto-updates.

## Layout

- **GitHub Releases** — every public release is published here as
  `<app>-v<version>` (e.g. `kandil-v1.0.1`), with the DMG, zip, and per-release
  `appcast.xml` attached as assets.
- **GitHub Pages (`gh-pages` branch)** — serves stable, per-app appcast URLs:
  - `https://kenopahq.github.io/releases/<app>/appcast.xml`
  - `https://kenopahq.github.io/releases/<app>/prerelease-appcast.xml` (optional)

  The `appcast.xml` on Pages lists every release of that app, with download URLs
  pointing at the binaries on this repo's GitHub Releases. Sparkle clients fetch
  the Pages URL to discover updates.

## Per-app structure (on the gh-pages branch)

```
/<app>/appcast.xml
/<app>/prerelease-appcast.xml   (optional)
/<app>/index.html               (optional landing page)
```

## Why a single repo for all apps?

- One GitHub Pages site to manage, one Cloudflare cache to warm, one place to
  audit security/distribution policy.
- DMG/zip on GitHub Releases scales to thousands of artifacts per repo with no
  per-app overhead.
- Source repos stay private; only signed/notarized binaries plus their appcast
  manifests are made public.

## How releases land here

Each app's private CI publishes here using the `KENOPAHQ_REPO_CONTENTS_RW_PAT`
secret (org-wide):

1. Create a GitHub Release on this repo tagged `<app>-v<version>` and attach the
   DMG, zip, and `appcast.xml` as assets.
2. Update `<app>/appcast.xml` on the `gh-pages` branch (the Sparkle feed) to
   include the new release entry.

See the source app's `.github/workflows/publish.yml` for the implementation.

## Adding a new app

No changes required here. The app's publish workflow creates the per-app
directory on `gh-pages` on its first release.
