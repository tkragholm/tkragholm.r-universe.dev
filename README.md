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

## When builds happen

Not on push. R-universe runs an **hourly cron** (`sync.yml` in
`r-universe-org/control-room`) that checks every registered monorepo for changes to
the registry *and* to the package sources, then dispatches a build. There is no
webhook, so pushing here to "nudge" a source change does nothing the same cron
would not have done within the hour.

A separate nightly job retries anything that failed a source or Windows/macOS
r-release binary in the last five days, and fully rebuilds packages older than
thirty days.

The sync aborts wholesale while GitHub is degraded, so several cycles in a row can
silently do nothing during an incident. Before assuming a setup mistake, check:

```sh
gh run list -R r-universe-org/control-room --workflow sync.yml
```

Build logs live in `r-universe/tkragholm`, which is read-only to us:

```sh
gh run view <id> -R r-universe/tkragholm --log-failed
```

The R-universe GitHub app still needs to be installed on this account — it grants
access, it just is not what triggers the build.
