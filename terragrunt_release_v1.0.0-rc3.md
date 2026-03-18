## 🎉 v1.0.0 Release Candidate

This is the third release candidate for Terragrunt 1.0.

This release covers the latest user-facing changes since the previous release candidate; try it out and share feedback in the [GitHub Discussion](https://github.com/gruntwork-io/terragrunt/discussions), and review the release candidate schedule in [The Road to 1.0: Release Schedule](https://www.gruntwork.io/blog/the-road-to-1-0-release-schedule).

## Release notes source data

```json
{
  "⚙️ Process Updates": [
    "Terragrunt’s internal option and configuration plumbing has been modernized to make the codebase easier to extend without relying on a single global options struct. This refactors many components to use dedicated option structs (for example `run.Options`, shell options constructors, provider-cache/backend/remote-state/tf/tflint/GCP option structs), introduces `configbridge` and `ParsingContext` construction improvements, and updates all callers/tests to use the new APIs.",
    "Terragrunt’s path normalization strategy was standardized so command execution and repo discovery behave consistently across operating systems and working directories. Most `filepath.Abs`/`filepath.ToSlash` usage was replaced with root-working-dir–relative joins and `filepath.Clean`, catalog repo creation now receives the root working directory instead of calling `os.Getwd`, and stack/discovery/utilities plus tests and integrations were updated accordingly.",
    "Remote state initialization behavior is now more configurable to better support Terragrunt and Terraform workflows with custom backend bootstrapping. `remote_state.disable_init` now skips Terragrunt backend bootstrap while still passing `-backend-config` arguments through to Terraform, with runner logic, fixtures, tests, and docs updated to match.",
    "Output and execution wiring has been simplified so components receive only the information they need, which reduces coupling and makes behavior easier to validate. Output handling was centralized in a `Writers` struct, `FlushOutput` was simplified, and regression coverage was added for detailed exit codes when using a Terraform source and `--working-dir`.",
    "Error handling was standardized to make failures easier to understand and test. Worker-pool error handling now uses `MultiError` consistently with tests verifying exact error counts, and retry/ignore error matching was tightened to match only cleaned stderr plus underlying errors while preserving `-` and `=` characters, with exported helpers and tests.",
    "Cloud-provider authentication and CLI helpers were refined so behavior is more predictable in complex environments. Terraform/OpenTofu helper APIs were refactored to avoid options imports, typed GCP credentials file handling was added, GCP impersonation now overrides base credentials, and IAM assume-role defaults were moved into a dedicated `iam` helper."
  ],
  "🏎️ Performance Improvements": [
    "Filter evaluation is now more efficient, improving performance for workflows that rely heavily on filtering and classification. Filter globs are compiled eagerly, classifier-match logging was removed, and filter expansion now returns errors that are propagated through updated callers and tests."
  ],
  "🐛 Bug Fixes": [
    "Terraform-related environment variable values are now handled more safely, preventing accidental interpolation issues in complex inputs. The implementation escapes `${...}` patterns in complex `TF_VAR` values while preserving literal strings, and adds tests to validate the behavior.",
    "Terragrunt now provides clearer guidance when users pass unknown flags to `run` commands, reducing time spent troubleshooting CLI usage. It adds special handling for undefined flags to show a passthrough hint and includes test coverage for the new behavior.",
    "Provider version reporting is now more accurate by ignoring invalid version strings instead of treating them as usable versions. The provider versions API now filters results to valid semver values and logs any skipped invalid entries for visibility.",
    "Discovery is more resilient in restricted or offline environments so failures are classified correctly. Any `*url.Error` returned from discovery is now treated as an offline condition, with a test covering blocked registry mirror scenarios.",
    "Path handling is more consistent across environments, reducing issues caused by relative or mismatched paths. Included config paths are normalized to absolute paths before tracking, and tests were added for relative includes and git-filter scenarios.",
    "Terragrunt produces more actionable diagnostics when hooks fail, making CI and local debugging easier. Hook failure logging now includes the command, exit code, and output details, tflint error wrapping was improved, and fixtures/tests were added to validate error reporting."
  ],
  "📖 Documentation Updates": [
    "The documentation has been updated to reflect recent CLI deprecations and completed behaviors, so guidance matches what the CLI actually does today. Specifically, the strict controls and CLI migration docs were revised for several deprecated flags/options, and related guides and links were refreshed to point at the current repos and GitHub Actions.",
    "Terragrunt’s documentation site has been prepared for the new docs domain so existing links continue to work during the transition. This includes redirects and schema updates for `docs.terragrunt.com`, removing legacy marketing UI, moving the Starlight docs into the `docs/` directory, and adding a permanent redirect from `/contact-tgs` to `terragrunt.com/contact-tgs`.",
    "The compatibility documentation and routes have been modernized so the URLs are more stable and easier to link to. In practice, compatibility endpoints were changed from query-parameter URLs to path-based endpoints, and the docs and routing were updated accordingly."
  ],
  "🛠️ Breaking Changes": [
    "Terragrunt now fails fast when a single directory contains both a standard configuration file and a stack configuration file, so projects must choose one format per folder. Concretely, the config loader detects `terragrunt.hcl` alongside `terragrunt.stack.hcl`, returns an explicit error, and the documentation was updated to describe this constraint."
  ],
  "🧹 Chores": [
    "Continuous integration workflows and dependencies were refreshed to keep builds secure and maintainable. This updates multiple GitHub Actions (including `actions/setup-go`, `actions/upload-artifact`, `actions/download-artifact`, `sigstore/cosign-installer`, `actions/stale`, DigiCert signing, and `jdx/mise-action`) and tunes CI memory settings where needed.",
    "Go module and JavaScript dependency sets were updated to incorporate recent fixes and keep development tooling current. This includes bumping several Go dependencies in `go.mod`/`go.sum` (including `sops` to v3.12.1 and `github.com/cloudflare/circl` to v1.6.3), updating `aws-sdk-go-v2` patches, and updating Starlight/docs JavaScript dependencies and lockfiles.",
    "Testing and helper utilities were simplified to reduce external coupling and improve maintainability. Terratest helpers were replaced with internal utilities, and related dependencies were removed or cleaned up.",
    "Developer tooling and lint configuration were adjusted to reduce false positives and improve consistency across platforms. The `lll` linter is now enabled selectively with long Go function signatures wrapped to satisfy line-length checks, LINT_TAGS discovery in the Makefile is restricted to Go source files, and Go-related GitHub Actions cache restore keys were made more OS- and `go.sum`-specific."
  ]
}

```