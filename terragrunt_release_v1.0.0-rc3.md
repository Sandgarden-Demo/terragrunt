## 🎉 v1.0.0 Release Candidate

This is the third release candidate for Terragrunt 1.0.

This release includes updates and fixes identified during release candidate testing; feedback in the GitHub Discussions forum at https://github.com/gruntwork-io/terragrunt/discussions is recommended to help finalize 1.0.

The release candidate schedule is documented in https://www.gruntwork.io/blog/the-road-to-1-0-release-schedule.

{
  "⚙️ Process Updates": [
    "Terragrunt updated the release build and repository automation to keep CI and dependency workflows current. This includes updates to multiple GitHub Actions used for builds, artifact upload/download, code signing, and tooling setup to pick up upstream fixes and security patches.",
    "Terragrunt refreshed several upstream dependencies used in development and tests to incorporate recent patches. This includes updates across Go modules (e.g., AWS SDK modules, SOPS, and test dependencies) and the JavaScript dependencies used for the documentation toolchain."
  ],
  "🐛 Bug Fixes": [
    "The `run` command now handles unknown flags more helpfully instead of failing with a confusing parser error. When Terragrunt detects an undefined flag in a `run` context, it provides a passthrough hint and includes test coverage for the behavior.\n\n```bash\n# Example (representative)\n$ terragrunt run -- -some-tool-flag\n12:34:56.789 ERROR  Unknown flag \"-some-tool-flag\" for terragrunt run; pass through flags after \"--\" (e.g. \"terragrunt run -- -some-tool-flag\").\n```",
    "Terragrunt now reports hook failures with clearer, more actionable details instead of obscuring whether a command failed to start or exited non-zero. Hook error handling now includes the command, exit code, and output details, and it also tightens related error wrapping (including `tflint`) with new fixtures and tests.\n\n```bash\n# Example output (representative)\n11:39:03.648 INFO   Executing hook: before_hook\n11:39:03.649 ERROR  Hook \"before_hook\" failed (command: bash -c exit 1) with exit code 1\n11:39:03.649 ERROR  Hook output:\n\u003ccommand output here\u003e\n```",
    "Complex `TF_VAR_*` values that include `${...}` patterns no longer break variable handling in edge cases. Terragrunt now escapes `${...}` sequences where appropriate while preserving literal values, and it adds tests to ensure variable expansion behaves predictably.",
    "Remote state initialization now respects `remote_state.disable_init` in Terragrunt + Terragrunt/Terraform/Terragrunt-backend workflows that previously performed unwanted bootstrap work. Terragrunt skips the Terragrunt backend bootstrap while still passing backend configuration arguments through to Terraform, and it updates runner logic, fixtures, tests, and docs to match the corrected behavior.",
    "Discovery now treats certain network failures as offline in a way that better matches real-world restricted environments. When discovery encounters a `*url.Error` (e.g., caused by blocked registry mirrors), Terragrunt classifies the scenario as offline and verifies the behavior with a dedicated test.",
    "The provider versions API no longer returns invalid version strings that can confuse tooling that expects semantic versions. Terragrunt now filters provider versions down to valid semver values and logs any skipped invalid entries so you can diagnose upstream registry data issues.",
    "Terragrunt now handles worktree diff filter expansion failures explicitly instead of continuing after silent or partial filter processing. The filter-glob expansion path now compiles globs eagerly and returns errors to callers (with updated tests), which prevents confusing “no matches” behavior when the underlying filter input is invalid.",
    "Terragrunt now normalizes included configuration paths consistently before it tracks them, which avoids mismatches when includes use relative paths. The include tracking logic converts include paths to absolute paths up front and adds a regression test that covers relative include cases.",
    "Provider cache usage during dependency output resolution now behaves more reliably in scenarios where Terragrunt previously bypassed the cache unexpectedly. Terragrunt adds fixtures and an integration test that verify it uses the provider cache server when resolving dependency outputs, preventing surprise network fetches in constrained environments.",
    "Terragrunt’s retry/ignore error matching now compares errors more accurately to reduce false positives and false negatives. The matcher now operates on cleaned `stderr` plus the underlying error (while preserving `-` and `=` characters), and it exports helper functions with tests to ensure stable matching behavior."
  ],
  "📖 Documentation Updates": [
    "The documentation site now targets `docs.terragrunt.com` and includes redirects and schema updates so existing links keep working. This update also removes legacy marketing UI artifacts from the docs build and reorganizes the Starlight docs project so the docs content and tooling live under the `docs/` directory."
  ],
  "🛠️ Breaking Changes": [
    "Terragrunt now enforces a single configuration file type per directory to prevent ambiguous behavior and hard-to-debug runs. When a folder contains both `terragrunt.hcl` and `terragrunt.stack.hcl`, Terragrunt fails fast with an explicit error and the documentation now calls out this constraint.\n\n```bash\n# Directory layout (now invalid)\n$ ls\nterragrunt.hcl\nterragrunt.stack.hcl\n\n# Example failure (representative)\n12:34:56.789 ERROR  Found both \"terragrunt.hcl\" and \"terragrunt.stack.hcl\" in the same directory; Terragrunt requires exactly one config type per directory.\n```",
    "Terragrunt changed how it normalizes and constructs file paths so that it consistently treats paths as rooted at the repository (root working directory) instead of relying on absolute-path conversions and slash normalization. This update replaces many uses of `filepath.Abs`/`filepath.ToSlash` with root-working-dir–relative joins plus `filepath.Clean`, and it updates discovery, stack/unit loading, catalog repo creation, and related utilities and tests to align with the new normalization behavior.\n\n```bash\n# Migration guidance: if your automation depended on absolute paths, prefer\n# constructing paths from a known root instead of expecting Terragrunt to\n# convert/normalize paths implicitly.\n#\n# Example pattern (shell):\n$ REPO_ROOT=\"$(git rev-parse --show-toplevel)\"\n$ terragrunt run-all plan --working-dir \"$REPO_ROOT/live\"\n```"
  ],
  "🧹 Chores": [
    "Terragrunt tightened lint configuration so line-length enforcement works consistently without forcing broad formatting churn across the codebase. The build now enables the `lll` linter more selectively and wraps long Go function signatures where needed to satisfy line-length checks."
  ]
}

