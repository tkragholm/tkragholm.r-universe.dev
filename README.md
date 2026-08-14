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
