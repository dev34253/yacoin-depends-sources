# YACoin depends sources

Mirror of the source tarballs that the `depends/` build system of [dev34253/yacoin](https://github.com/dev34253/yacoin) pins. The files are
the assets of the release [`sources`](https://github.com/dev34253/yacoin-depends-sources/releases/tag/sources); `depends/Makefile` names
that release as `FALLBACK_DOWNLOAD_PATH`, so a build fetches a file from here when its original host fails.

- Each asset is stored under the recipe's `$(package)_file_name`, byte for byte as the original host served it. The recipe's
  `$(package)_sha256_hash` is checked after every download, so this mirror cannot change what is built.
- `SHA256SUMS` lists every asset with its SHA-256.
- The code repository checks, on every change to `depends/`, that every source the recipes pin is here with the pinned hash.

## Adding a file

When a recipe pins a new source, add it before the recipe change is merged:

```sh
make -C depends download SOURCES_PATH=/tmp/sources    # in the code repository; checks every hash
gh release upload sources /tmp/sources/<file_name> --repo dev34253/yacoin-depends-sources
```

Then regenerate `SHA256SUMS` from all assets and upload it with `--clobber`. If `gh release create` or `upload` loses its connection, the
release can be left a draft, whose asset URLs answer 404: check with `gh release view sources --json isDraft` and upload what is missing. Files that no recipe pins any more stay, so older commits
still build.
