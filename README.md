# eider-markdown

A CommonMark subset plus GFM tables for [Eider](https://github.com/klxdev/eider-lang): a block tree and a safe HTML renderer (raw HTML never gets through). Pure Eider; it needs no capability.

- ATX and setext headings, paragraphs, emphasis, code spans, links, autolinks, images,
  lists, block quotes (with `**Note:**`/`**Warning:**`/`**Tip:**` callouts), thematic
  breaks, fenced code blocks and GFM tables.
- `markdown.to_html(bytes)` for a whole document; `parse` then `html` with `Options` to
  rewrite link destinations or highlight code; `links`, `headings`, `code_blocks`, `title`
  and `summary` list what a document has.
- Every `<`, `>`, `&`, `"` and `'` of the source is escaped; `javascript:`, `vbscript:` and
  `data:` destinations render as text.

It is the subset eider-lang's documentation site uses (its `docs/site/CONTRACT.md`).

Used by: eider-lang (eider-doc).

## Using it

In your package's `eider.toml`:

```toml
[deps.markdown]
git = "https://github.com/klxdev/eider-markdown.git"
version = "0.1.0"
# pure: granted nothing (no `uses`)
```

then `eider fetch` and `import markdown`.

## Developing

```sh
sh scripts/build.sh     # fetch the dependencies, check every module
sh scripts/test.sh      # build, then run tests/*.ei against their .out files
EIDER_TOOLCHAIN=../eider-lang sh scripts/test.sh   # the same, against an eider-lang checkout
```

The scripts use the toolchain [`eider`](https://github.com/klxdev/eider-lang/blob/main/docs/site/tooling/eider.md)
picks: `$EIDER_TOOLCHAIN` (an installed toolchain's directory, or an eider-lang checkout for
unreleased work), else the installed release for `eider.toml`'s `eider = "0.1"` (what
`eider toolchain show` prints here). Install `eider` and the latest toolchain with
eider-lang's `install.sh --toolchain` (from its latest release); without either, the
scripts stop with exit 2 and say so. The toolchain's `VERSION` must have this package's
MAJOR.MINOR (the `eider` field of `eider.toml`), else they stop with exit 2 and say which
release to use. eiderc and eider-pkg run through `eider` (`eider eiderc ...`, `eider eider-pkg
...`, with that toolchain), or directly when `eider` is not on the PATH. It has no dependencies beyond the toolchain's `core` and `std`. eider-lang pins a release of it in its `vendor.lock` and fetches it into `.vendor/markdown` for its own tools (`scripts/vendor.sh`, which checks the content hash); that copy must stay byte-identical to a tagged release here.

eider-lang's `scripts/verify.sh` builds and tests this repository against its own checkout
when it is next to it (`scripts/check-tools.sh`).

## Versions

`VERSION` and `eider.toml` hold this package's version (semver). Before 1.0 its MAJOR.MINOR
follows the Eider toolchain it goes with (`eider = "0.1"`) and its PATCH is its own;
CHANGELOG.md lists the releases. A release is tagged `v<version>`, which is what
`version = "..."` in a dependent's `eider.toml` resolves to.
