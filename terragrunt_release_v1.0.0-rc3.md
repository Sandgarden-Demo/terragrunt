## 🎉 v1.0.0 Release Candidate

This is the third release candidate for Terragrunt 1.0.

This release covers a breaking change in how Terragrunt loads unit vs stack configuration files, plus bug fixes for hook failure diagnostics, `run` flag errors, provider version discovery, include path tracking, and offline discovery. Try it out and share feedback in the [GitHub Discussion](https://github.com/gruntwork-io/terragrunt/discussions), and review the release candidate schedule in [The Road to 1.0: Release Schedule](https://www.gruntwork.io/blog/the-road-to-1-0-release-schedule).

## 🛠️ Breaking Changes

### `terragrunt.hcl` and `terragrunt.stack.hcl` cannot coexist in the same directory

Terragrunt errors when a single directory contains both `terragrunt.hcl` and `terragrunt.stack.hcl`, so you must choose one configuration format per folder.

e.g.

```bash
# Invalid: both file types in the same folder
./live/prod/app/terragrunt.hcl
./live/prod/app/terragrunt.stack.hcl

# Fix: split them into separate folders (one format per folder)
./live/prod/app/terragrunt.hcl
./live/prod/app-stack/terragrunt.stack.hcl
```

## 📖 Documentation Updates

### Guidance updated for recent CLI deprecations

The strict controls and CLI redesign docs now reflect deprecated flags and current CLI behavior.

### Site prepared for the `docs.terragrunt.com` domain transition

The documentation site now includes redirects and schema updates for `docs.terragrunt.com`.

### Compatibility documentation updated for path-based routes

The compatibility docs and routes now use path-based endpoints instead of query parameters.

## 🐛 Bug Fixes

### `${...}` expansion avoided in complex `TF_VAR_*` values

A bug caused `${...}` patterns in complex `TF_VAR_*` values to expand unexpectedly, and Terragrunt now escapes those patterns.

### More actionable diagnostics when hooks fail

Hook failures previously omitted key execution details, and Terragrunt now logs the hook command, exit code, and output.

### Clearer guidance for unknown flags passed to `run`

Unknown flags passed to `run` previously produced unhelpful errors, and Terragrunt now prints a passthrough hint.

### Provider version discovery ignores invalid version strings

Provider version discovery previously accepted invalid version strings, and Terragrunt now ignores non-semver values.

### Discovery treats `*url.Error` as an offline condition

Discovery previously misclassified `*url.Error` failures, and Terragrunt now treats them as an offline condition.

### Include path tracking uses normalized absolute paths

Relative include paths previously caused inconsistent include tracking, and Terragrunt now records normalized absolute paths.

## 🧹 Chores

### CI workflows and dependencies refreshed

Continuous integration workflows and dependencies were refreshed to keep builds secure and maintainable.

### Go modules and docs-site dependencies updated

Go module and JavaScript dependency sets were updated to incorporate recent fixes and keep development tooling current.

### Terratest helpers replaced with internal utilities

Testing and helper utilities were simplified to reduce external coupling and improve maintainability.

### Lint and developer tooling configuration refined

Developer tooling and lint configuration were adjusted to reduce false positives and improve consistency across platforms.

## 🏎️ Performance Improvements

### More efficient filter evaluation

Terragrunt evaluates filters more efficiently, improving performance for workflows that rely heavily on filtering and classification.

## ⚙️ Process Updates

### Options and configuration plumbing refactored

Terragrunt refactored option handling to use dedicated option structs (e.g., `run.Options`) instead of a single global options struct.

### Standardized path normalization strategy

Terragrunt standardized path normalization to reduce `filepath.Abs`/`filepath.ToSlash` usage in favor of root-working-dir–relative joins and `filepath.Clean`.

### More configurable remote state initialization

`remote_state.disable_init` now skips Terragrunt backend bootstrapping while still passing `-backend-config` arguments through to Terraform.

### Output and execution wiring simplified

Terragrunt centralized output handling in `Writers`, reducing coupling by passing only required execution and output context to components.

### Error handling standardized

Terragrunt standardized worker-pool and retry error handling around `MultiError` and stricter stderr matching.

### Refined cloud-provider auth and CLI helpers

GCP and IAM credential helpers now behave more predictably, including GCP impersonation overriding base credentials.