# Headstart — update feed

One file, `version.json`, that every installed Headstart panel reads when it
opens. If the `version` in it is newer than the copy the editor is running, the
panel shows a bar at the top with the `notes` and a Download button pointing at
`url`.

This folder is meant to live in its own **public** repository — the panel has to
read it without a login, while the panel's own code stays private. Nothing here
is part of the panel; it ships nowhere.

## Announcing a new build

1. Build the zips (`install/Build ZIP.command`) and attach them to a release
   **on this repository**, which is public, so anyone can download them without
   signing in:

   ```bash
   gh release create v1.1.0 --repo genxnayem-svg/Headstart-updates \
       --title "Headstart 1.1.0" --notes "What changed." \
       Headstart-1.1.0-mac.zip Headstart-1.1.0-win.zip
   ```
2. Edit `version.json`: bump `version`, set today's `date`, write a line or two
   in `notes`. Leave `url` alone — it already points at the newest release.
3. Commit and push. Every panel picks it up the next time it opens.

`version` must be plain numbers like `1.2.0` — the panel compares them part by
part. Keep it the same as `ExtensionBundleVersion` in the panel's
`CSXS/manifest.xml`, which is the number the panel compares against.

`url` is `.../Headstart-updates/releases/latest`, which always resolves to the
newest release here, so step 2 only has to touch it if the downloads ever move
somewhere else.

## Where the panel reads it from

`main.js`, constant `UPDATE_FEED_URL`. It is currently

```
https://raw.githubusercontent.com/genxnayem-svg/Headstart-updates/main/version.json
```

so the public repository has to be called `Headstart-updates` with this
`version.json` at its root on `main` — or change that constant to match
wherever you actually put it.

The panel fails silently: no network, no repository, broken JSON, nothing shows
and the panel works as normal. It never blocks anything and it never downloads
or installs by itself — the editor clicks the link and runs the installer.
