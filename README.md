# URAH releases for Knights of Honor II: Sovereign

Release files of the URAH mods, built and signed by GitHub Actions. No source code here: the
sources are in the `URAH_koh2_*` repos of [URAHDangerzone](https://github.com/URAHDangerzone).

| Release | What it is |
|---|---|
| [`URAH_Starter-<date>-<commit>`](https://github.com/URAHDangerzone/URAH_koh2_releases/releases?q=URAH_Starter&expanded=true) | **start here**: BepInEx 5 + URAH_Loader + URAH_Net + URAH_PluginManager, unzip into the game folder, see `INSTALL.md` inside |
| [`URAH_PluginManager-v<version>`](https://github.com/URAHDangerzone/URAH_koh2_releases/releases?q=URAH_PluginManager&expanded=true), [`URAH_Loader-v<version>`](https://github.com/URAHDangerzone/URAH_koh2_releases/releases?q=URAH_Loader&expanded=true), [`URAH_Net-v<version>`](https://github.com/URAHDangerzone/URAH_koh2_releases/releases?q=URAH_Net&expanded=true), [`URAH_Hover-v<version>`](https://github.com/URAHDangerzone/URAH_koh2_releases/releases?q=URAH_Hover&expanded=true) | one plugin as `BepInEx/plugins/URAH_<Name>/`; normally installed and updated in the game by URAH_PluginManager |
| [`URAH_HRE-v<version>`](https://github.com/URAHDangerzone/URAH_koh2_releases/releases?q=URAH_HRE&expanded=true), [`URAH_RoyalWedding-v<version>`](https://github.com/URAHDangerzone/URAH_koh2_releases/releases?q=URAH_RoyalWedding&expanded=true) | `<Mod>-plugins.zip`: the signed `plugins/` folder of the mod (`manifest.txt`, `manifest.sig`, the plugin DLLs), shipped inside the Workshop mod |

## Plugin manager

[URAH_PluginManager](https://github.com/URAHDangerzone/URAH_koh2_pluginmanager) (in the starter) adds
**Settings → Plugins (URAH)** to the game: it switches the standalone plugins (URAH_Net, URAH_Hover, ...)
on and off without a restart, and installs, updates and removes them. Plugins that come with a
Workshop mod are not listed there; URAH_Loader handles them.

It reads [`index.txt`](index.txt) in this repo, the list of the newest release of each standalone
plugin, and only when the player clicks "Fetch plugins". The release workflow rewrites it with every
plugin release and signs it into [`index.sig`](index.sig) with the same key as the releases. Each
entry names the release zip and the SHA-256 of its `SHA256SUMS`; the manager checks the signature,
then `SHA256SUMS`, then every file before it writes anything. `seq` goes up with every index, and the
manager refuses an older one than it has seen.

## Checking the signatures

`URAH_Loader` only loads plugin DLLs listed in a `manifest.txt` whose `manifest.sig` matches the
URAH release key (RSA-3072, PKCS#1 v1.5 over SHA-256). The other releases carry a `SHA256SUMS` signed
the same way, and so is `index.txt`. Check any of them with the public key [`release_key.pem`](release_key.pem):

```
openssl dgst -sha256 -verify release_key.pem -signature manifest.sig manifest.txt
openssl dgst -sha256 -verify release_key.pem -signature SHA256SUMS.sig SHA256SUMS && sha256sum -c SHA256SUMS
openssl dgst -sha256 -verify release_key.pem -signature index.sig index.txt
```

The plugin builds are reproducible: building the plugin repo at the commit named in the release notes,
with `external/URAH_shared` at the URAH_koh2_shared commit named there and the same .NET SDK
(`global.json`), gives the same SHA-256 as in `manifest.txt` / `SHA256SUMS`.
