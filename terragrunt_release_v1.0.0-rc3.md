## 🎉 v1.0.0 Release Candidate

This is the third release candidate for Terragrunt 1.0.

This release includes two breaking changes and several stability fixes identified during release candidate testing, with additional updates to documentation and release automation; sharing feedback in [GitHub Discussions](https://github.com/gruntwork-io/terragrunt/discussions) is recommended to help finalize 1.0, and the overall release candidate timeline is documented in [The Road to 1.0: Release Schedule](https://www.gruntwork.io/blog/the-road-to-1-0-release-schedule).

## 🛠️ Breaking Changes

### Configuration discovery enforces a single file type per directory

Configuration discovery now fails fast when a directory contains both `terragrunt.hcl` and `terragrunt.stack.hcl`.
Previously, this layout could produce ambiguous behavior during stack and unit loading.
To migrate, move one of the configuration files into a different directory (or remove the unused file) so each directory contains exactly one Terragrunt configuration file type.

e.g.

```bash
$ ls
terragrunt.hcl
terragrunt.stack.hcl

12:34:56.789 ERROR  Found both "terragrunt.hcl" and "terragrunt.stack.hcl" in the same directory; Terragrunt requires exactly one config type per directory.
```

### Path normalization consistently roots joins at the repository working directory

Path normalization now builds paths from the repository working directory using root-relative joins plus `filepath.Clean`, instead of relying on `filepath.Abs` and forward-slash normalization.
This change affects workflows that relied on Terragrunt implicitly converting relative paths into absolute paths or normalizing path separators.
If automation depends on absolute paths, compute absolute paths explicitly and pass them into Terragrunt (for example via `--working-dir`) instead of relying on implicit conversion.

e.g.

```bash
$ REPO_ROOT="$(git rev-parse --show-toplevel)"
$ terragrunt run-all plan --working-dir "$REPO_ROOT/live"
```

## 📖 Documentation Updates

### Documentation site migrated to `docs.terragrunt.com`

The documentation site now targets `docs.terragrunt.com` and includes redirects and schema updates to keep existing links working.
This update also reorganizes the Starlight documentation project so documentation content and tooling live under `docs/`.

## 🐛 Bug Fixes

### `run` provides a clear passthrough hint for unknown flags

The `run` command previously failed with a confusing parser error when an undefined flag appeared in a `run` context.
The command now reports the unknown flag and explains how to pass through tool-specific flags after `--`.

e.g.

```bash
$ terragrunt run -- -some-tool-flag
12:34:56.789 ERROR  Unknown flag "-some-tool-flag" for terragrunt run; pass through flags after "--" (e.g. "terragrunt run -- -some-tool-flag").
```

### Hook failures report clearer execution vs exit-code errors

Hook failures previously obscured whether a hook command failed to start or started successfully but exited non-zero.
Hook error handling now includes the executed command, exit code, and output details to make failures easier to diagnose.

e.g.

```bash
11:39:03.648 INFO   Executing hook: before_hook
11:39:03.649 ERROR  Hook "before_hook" failed (command: bash -c exit 1) with exit code 1
11:39:03.649 ERROR  Hook output:
<command output here>
```

### `TF_VAR_*` handling preserves literal `${...}` sequences

Complex `TF_VAR_*` values that included `${...}` patterns could break variable handling in edge cases.
Variable processing now escapes `${...}` sequences where required while preserving literal values.

### `remote_state.disable_init` correctly prevents backend bootstrap work

Remote state initialization previously performed unwanted bootstrap work even when `remote_state.disable_init` specified that behavior.
Remote state handling now skips Terragrunt backend bootstrap while still passing backend configuration arguments through to the underlying IaC engine.

### Discovery classifies `*url.Error` network failures as offline

Discovery behavior previously treated some network failures as online, which caused confusing behavior in restricted environments.
Discovery now classifies `*url.Error` failures (such as blocked registry mirrors) as offline to better match real-world network constraints.

### Provider versions API filters invalid semantic versions

The provider versions API previously returned invalid version strings that could confuse tooling that expects semantic versions.
The API now filters provider versions down to valid semver values and logs skipped invalid entries to support diagnosis of upstream registry data.

### Worktree diff filter expansion fails fast on invalid glob input

Worktree diff filter expansion previously continued after silent or partial filter processing failures.
Filter-glob expansion now compiles globs eagerly and returns errors to prevent confusing “no matches” results when filter input is invalid.

### Include tracking consistently normalizes relative include paths

Include tracking previously mixed relative and absolute include paths, which could cause mismatches in tracking.
Include tracking now converts include paths to absolute paths up front to ensure consistent tracking.

### Provider cache usage remains consistent during dependency output resolution

Dependency output resolution previously bypassed the provider cache server in some scenarios.
Dependency output resolution now uses the provider cache server consistently to avoid unexpected network fetches in constrained environments.

### Retry and ignore error matching compares stderr and error values more accurately

Retry and ignore error matching previously produced false positives and false negatives in some cases.
Error matching now compares a cleaned `stderr` value alongside the underlying error while preserving `-` and `=` characters.

## 🧹 Chores

### `lll` linter configuration tightened for consistent line-length enforcement

Lint configuration now enforces line-length limits more consistently without requiring broad formatting churn.
The build enables the `lll` linter more selectively and wraps long Go function signatures where required.

## ⚙️ Process Updates

### Release automation and dependency workflows updated

Release build automation now uses updated GitHub Actions for builds, artifact upload/download, code signing, and tooling setup to incorporate upstream fixes and security patches.
Development and test dependencies (including the documentation toolchain) were also updated to incorporate recent patches.

