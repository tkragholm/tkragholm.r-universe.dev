# tkragholm.r-universe.dev

Registry for <https://tkragholm.r-universe.dev>.

`packages.json` lists the R packages R-universe should build. Both live in the
[sas7bdat-parser-rs](https://github.com/tkragholm/sas7bdat-parser-rs) monorepo,
hence the `subdir` field on each entry.

| Package | Does |
|---------|------|
| `fastsas` | Reads `.sas7bdat` files into R, with `haven`-compatible column types. |
| `fastsasconvert` | Converts `.sas7bdat` trees to Parquet/CSV/TSV without the data entering R. |

## Installing

```r
install.packages(
  c("fastsas", "fastsasconvert"),
  repos = c("https://tkragholm.r-universe.dev", "https://cloud.r-project.org")
)
```

R-universe serves prebuilt binaries for Windows, macOS and Linux, so users do not
need a Rust toolchain. Building from source does — see `SystemRequirements` in each
package.

## Adding a package

Append an entry to `packages.json`. `package` must match the `Package:` field in
that package's `DESCRIPTION`, `url` is any public git remote, and `subdir` is
needed only when the package is not at the repository root. A `branch` field can
pin a branch or tag.

R-universe rebuilds on push, via the R-universe GitHub app installed on this
account.
