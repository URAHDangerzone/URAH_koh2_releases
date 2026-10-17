# URAH releases for Knights of Honor II: Sovereign

Release files of the URAH mods, built and signed by GitHub Actions. No source code here: the
sources are in the `URAH_koh2_*` repos of [URAHDangerzone](https://github.com/URAHDangerzone).

| Release | What it is |
|---|---|
| `URAH_HRE-v<version>`, `URAH_RoyalWedding-v<version>` | `<Mod>-plugins.zip`: the signed `plugins/` folder of the mod (`manifest.txt`, `manifest.sig`, the plugin DLLs), shipped inside the Workshop mod |

`URAH_Loader` only loads plugin DLLs listed in a `manifest.txt` whose `manifest.sig` matches the
URAH release key (RSA-3072, PKCS#1 v1.5 over SHA-256). Check one yourself with the public key from
[`release_key.pem`](release_key.pem):

```
openssl dgst -sha256 -verify release_key.pem -signature manifest.sig manifest.txt
```

The plugin builds are reproducible: building the plugin repo at the commit named in the release notes
with the same .NET SDK gives the same SHA-256 as in `manifest.txt`.
