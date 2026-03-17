## 🎉 v1.0.0 Release Candidate

This is the third release candidate for Terragrunt 1.0.

This release includes performance improvements, reliability fixes, and several user-facing bug fixes since RC2; please test it in your workflows and share feedback in the GitHub Discussions forum at https://github.com/gruntwork-io/terragrunt/discussions, and review the release candidate timeline in [The Road to 1.0: Release Schedule](https://www.gruntwork.io/blog/the-road-to-1-0-release-schedule).

## 🐛 Bug Fixes

### Retries added for registry timeouts in Provider Cache Server

The Provider Cache Server now retries requests to OpenTofu/Terraform provider registries when it encounters timeouts, which reduces transient failures during initialization. This change improves reliability in network-constrained environments.

### Discoverability of `init-from-module` documentation improved

Terragrunt now makes the internal `init-from-module` command easier to discover in the documentation. This change reduces confusion when you reference the command from hooks.

### Over-warning on strict controls prevented

Terragrunt now avoids emitting warnings for controls that already completed when you run with `--strict-mode`. This change reduces noisy output during runs.

### `run_cmd` now emits stdout/stderr when included

Terragrunt now forwards stdout/stderr from the `run_cmd` HCL function when a unit includes it. This change restores expected output behavior for included configurations.

### Provider Cache Server integration with custom registries fixed

Terragrunt now integrates the Provider Cache Server correctly with custom registries. If you use custom provider registries, set `--provider-cache-registry-names` so Terragrunt can proxy requests correctly.

### `exclude.no_run` now respects explicit `false`

Terragrunt now respects `exclude.no_run = false` when you set it explicitly. This change prevents Terragrunt from treating an explicit `false` as an unset value.

### `--report-file` now works for single-unit runs

Terragrunt now generates a run report when you set `--report-file` even if you do not use `--all`. This change makes report generation consistent across run modes.

### Terragrunt no longer rewrites paths in log messages

Terragrunt now avoids rewriting paths to be relative in log messages by default. This change prevents confusing path output when Terragrunt streams OpenTofu/Terraform output and hook output.

### Provider Cache Server now supports absolute module URLs in registry self-discovery

Terragrunt now resolves module sources correctly when the Provider Cache Server discovers a remote registry that returns absolute URLs. This change fixes module downloads in that registry configuration.

### SOPS decryption race condition fixed

Terragrunt now synchronizes concurrent access to SOPS-decrypted secrets when you use `--auth-provider-cmd`. This change prevents intermittent authentication failures across environments.

### Version constraints in stack runs fixed

Terragrunt now enforces `terragrunt_version_constraint` and `terraform_version_constraint` consistently when you run a stack. This change restores version-constraint checks for stack workflows.

### Interrupt propagation to OpenTofu/Terraform improved

Terragrunt now propagates interrupt signals more reliably to OpenTofu/Terraform processes it starts. This change improves cancellation behavior for explicit user interrupts and context cancellation.

### Remote state configuration parsing improved

Terragrunt now parses remote state configurations (including common S3 formats) more consistently. This change reduces decode failures caused by type mismatches in configuration values.

### Invalid unit configurations now fail fast during discovery

Terragrunt now raises an explicit error when it encounters invalid HCL during discovery instead of silently excluding the unit with a warning. This change prevents incomplete runs caused by hidden configuration errors.

### Partial-parse configuration cache collisions fixed

Terragrunt now avoids incorrect cache collisions when you enable `--use-partial-parse-config-cache`. This change ensures Terragrunt reads cached configurations accurately.

### Engine output formatting refined

Terragrunt now presents engine output with cleaner formatting and clearer tool naming. Terragrunt now labels stdout/stderr entries with the `engine` tool name instead of `tofu`.

## 🏎️ Performance Improvements

### Discovery performance improved

Terragrunt now discovers and filters units and stacks more efficiently during runs. This change reduces unnecessary parsing work while preserving correct behavior for graph-based filters.

### `EncodeSourceVersion` execution sped up

Terragrunt now speeds up `EncodeSourceVersion` by optimizing directory traversal. This change reduces overhead when Terragrunt computes source versions.

## ⚙️ Process Updates

### Go bumped to `v1.26.0`

Terragrunt now builds with Go `v1.26.0`. This change aligns CI and release builds with the latest stable Go toolchain.

### OpenTofu/Terraform compatibility matrix updated

Terragrunt now tests compatibility continuously against OpenTofu `1.11.4` and Terraform `1.14.4` in CI. This change improves confidence in engine compatibility for the 1.0 release line.

### AWS SDK and gRPC dependencies updated

Terragrunt now updates AWS SDK and gRPC dependencies to pick up upstream bug fixes and security patches. This change reduces exposure to known issues in those dependencies.
