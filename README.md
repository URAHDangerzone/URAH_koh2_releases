# URAH releases for Knights of Honor II: Sovereign

Release files of the URAH mods, built and signed by GitHub Actions. No source code here: the
sources are in the `URAH_koh2_*` repos of [URAHDangerzone](https://github.com/URAHDangerzone).

| Release | What it is |
|---|---|
| `URAH_Starter-<date>-<commit>` | **start here**: BepInEx 5 + URAH_Loader + URAH_Net + URAH_Hover, unzip into the game folder, see `INSTALL.md` inside |
| `URAH_Loader-v<version>`, `URAH_Net-v<version>`, `URAH_Hover-v<version>` | one plugin as `BepInEx/plugins/URAH_<Name>/`, for updating a single plugin |
| `URAH_HRE-v<version>`, `URAH_RoyalWedding-v<version>` | `<Mod>-plugins.zip`: the signed `plugins/` folder of the mod (`manifest.txt`, `manifest.sig`, the plugin DLLs), shipped inside the Workshop mod |

`URAH_Loader` only loads plugin DLLs listed in a `manifest.txt` whose `manifest.sig` matches the
URAH release key (RSA-3072, PKCS#1 v1.5 over SHA-256). The other releases carry a `SHA256SUMS` signed
the same way. Check any of them with the public key [`release_key.pem`](release_key.pem):

```
openssl dgst -sha256 -verify release_key.pem -signature manifest.sig manifest.txt
openssl dgst -sha256 -verify release_key.pem -signature SHA256SUMS.sig SHA256SUMS && sha256sum -c SHA256SUMS
```

The plugin builds are reproducible: building the plugin repo at the commit named in the release notes
with the same .NET SDK gives the same SHA-256 as in `manifest.txt`.
