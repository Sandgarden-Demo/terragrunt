## 🎉 v1.0.0 Release Candidate

This is the third release candidate for Terragrunt 1.0.

This release covers the latest user-facing changes since the previous release candidate; try it out and share feedback in the [GitHub Discussion](https://github.com/gruntwork-io/terragrunt/discussions), and review the release candidate schedule in [The Road to 1.0: Release Schedule](https://www.gruntwork.io/blog/the-road-to-1-0-release-schedule).

## 🛠️ Breaking Changes

### `terragrunt.hcl` and `terragrunt.stack.hcl` cannot coexist in the same directory

Terragrunt now fails fast when a single directory contains both `terragrunt.hcl` and `terragrunt.stack.hcl`, so you must choose one configuration format per folder.

## 🐛 Bug Fixes

### `${...}` expansion avoided in complex `TF_VAR_*` values

Terraform-related environment variable values are now handled more safely, preventing accidental interpolation issues in complex inputs.

### More actionable diagnostics when hooks fail

Terragrunt produces more actionable diagnostics when hooks fail, making CI and local debugging easier.

### Clearer guidance for unknown flags passed to `run`

Terragrunt now provides clearer guidance when users pass unknown flags to `run` commands, reducing time spent troubleshooting CLI usage.

### Provider version discovery ignores invalid version strings

Provider version reporting is now more accurate by ignoring invalid version strings instead of treating them as usable versions.

### Discovery treats `*url.Error` as an offline condition

Discovery is more resilient in restricted or offline environments so failures are classified correctly.

### Include path tracking uses normalized absolute paths

Path handling is more consistent across environments, reducing issues caused by relative or mismatched paths.

## 📖 Documentation Updates

### Guidance updated for recent CLI deprecations

The documentation has been updated to reflect recent CLI deprecations and completed behaviors, so guidance matches what the CLI actually does today.

### Site prepared for the `docs.terragrunt.com` domain transition

Terragrunt’s documentation site has been prepared for the new docs domain so existing links continue to work during the transition.

### Compatibility documentation updated for path-based routes

The compatibility documentation and routes have been modernized so the URLs are more stable and easier to link to.

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

Filter evaluation is now more efficient, improving performance for workflows that rely heavily on filtering and classification.

## ⚙️ Process Updates

### Options and configuration plumbing refactored

Terragrunt’s internal option and configuration plumbing has been modernized to make the codebase easier to extend without relying on a single global options struct.

### Standardized path normalization strategy

Terragrunt’s path normalization strategy was standardized so command execution and repo discovery behave consistently across operating systems and working directories.

### More configurable remote state initialization

Remote state initialization behavior is now more configurable to better support Terragrunt and Terraform workflows with custom backend bootstrapping.

### Output and execution wiring simplified

Output and execution wiring has been simplified so components receive only the information they need, which reduces coupling and makes behavior easier to validate.

### Error handling standardized

Error handling was standardized to make failures easier to understand and test.

### Refined cloud-provider auth and CLI helpers

Cloud-provider authentication and CLI helpers were refined so behavior is more predictable in complex environments.