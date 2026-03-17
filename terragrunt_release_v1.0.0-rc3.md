## 🎉 v1.0.0 Release Candidate

This is the third release candidate for Terragrunt 1.0.

This release includes performance improvements, reliability fixes, and several user-facing bug fixes since RC2; please test it in your workflows and share feedback in the GitHub Discussions forum at https://github.com/gruntwork-io/terragrunt/discussions, and review the release candidate timeline in [The Road to 1.0: Release Schedule](https://www.gruntwork.io/blog/the-road-to-1-0-release-schedule).

## 🐛 Bug Fixes

### Retries added for registry timeouts in Provider Cache Server

The Provider Cache Server now retries requests to OpenTofu/Terraform provider registries when it encounters timeouts to reduce transient initialization failures.

### Discoverability of `init-from-module` documentation improved

Terragrunt now makes the internal `init-from-module` command easier to discover in the documentation to reduce confusion when you reference it from hooks.

### Over-warning on strict controls prevented

Terragrunt no longer emits warnings for controls that already completed when you run with `--strict-mode`.

### `run_cmd` now emits stdout/stderr when included

Terragrunt now forwards stdout/stderr from the `run_cmd` HCL function when a unit includes it.

### Provider Cache Server integration with custom registries fixed

Terragrunt now proxies requests to custom provider registries correctly when you set `--provider-cache-registry-names`.

### `exclude.no_run` now respects explicit `false`

Terragrunt now respects `exclude.no_run = false` when you set it explicitly.

### `--report-file` now works for single-unit runs

Terragrunt now generates a run report when you set `--report-file` even when you run without `--all`.

### Terragrunt no longer rewrites paths in log messages

Terragrunt no longer rewrites paths to be relative in log messages by default.

### Provider Cache Server now supports absolute module URLs in registry self-discovery

The Provider Cache Server now resolves module sources correctly when registry self-discovery returns absolute URLs.

### SOPS decryption race condition fixed

Terragrunt now synchronizes concurrent access to SOPS-decrypted secrets when you use `--auth-provider-cmd` to prevent intermittent authentication failures.

### Version constraints in stack runs fixed

Terragrunt now enforces `terragrunt_version_constraint` and `terraform_version_constraint` when you run a stack.

### Interrupt propagation to OpenTofu/Terraform improved

Terragrunt now propagates interrupt signals more reliably to OpenTofu/Terraform processes it starts.

### Remote state configuration parsing improved

Terragrunt now parses remote state configurations more consistently, including common S3 formats, to reduce decode failures.

### Invalid unit configurations now fail fast during discovery

Terragrunt now returns an explicit error when it encounters invalid HCL during discovery instead of silently excluding the unit.

### Partial-parse configuration cache collisions fixed

Terragrunt now avoids cache collisions when you enable `--use-partial-parse-config-cache`.

### Engine output formatting refined

Terragrunt now presents engine output with cleaner formatting and labels stdout/stderr entries with `engine` instead of `tofu`.

## 🏎️ Performance Improvements

### Discovery performance improved

Terragrunt now discovers and filters units and stacks more efficiently during runs to reduce unnecessary parsing.

### `EncodeSourceVersion` execution sped up

Terragrunt now speeds up `EncodeSourceVersion` by optimizing directory traversal.

## ⚙️ Process Updates

### Go bumped to `v1.26.0`

Terragrunt now builds with Go `v1.26.0`.

### OpenTofu/Terraform compatibility matrix updated

Terragrunt now tests continuously against OpenTofu `1.11.4` and Terraform `1.14.4` in CI.

### AWS SDK and gRPC dependencies updated

Terragrunt now updates AWS SDK and gRPC dependencies to pick up upstream bug fixes and security patches.
