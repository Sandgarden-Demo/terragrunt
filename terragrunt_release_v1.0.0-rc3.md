## 🎉 v1.0.0 Release Candidate

This release includes stability-focused fixes and refinements ahead of Terragrunt 1.0, and we recommend trying this release candidate and sharing feedback in the [GitHub Discussions](https://github.com/gruntwork-io/terragrunt/discussions) thread(s). You can also review the overall timeline and expectations in [The Road to 1.0: Release Schedule](https://www.gruntwork.io/blog/the-road-to-1-0-release-schedule).

<!--
SOURCE ({
  "Improvements": [
    "Running with certain remote state settings now avoids unnecessary setup work while preserving correct backend behavior. Specifically, remote_state.disable_init now skips Terragrunt backend bootstrap while still passing backend-config arguments through to Terraform, and the runner logic, tests, fixtures, and documentation were updated accordingly.",
    "File and working-directory handling is now more consistent across platforms and environments. The implementation replaces many uses of filepath.Abs/ToSlash with root-working-dir–relative joins plus filepath.Clean, passes the root working directory into catalog repo creation instead of using os.Getwd, and updates discovery/stack/path utilities plus tests and integrations for the new normalization rules.",
    "Command output handling is now more consistent and less tightly coupled to global options. Output processing is centralized in a Writers struct and the engine/shell/Terraform/config parsing layers now use dedicated engine and run option structs rather than passing full TerragruntOptions.",
    "Configuration parsing and execution options are now more modular and easier to reason about. This refactors option passing by extracting provider-cache, backend, remote-state, run, tf, tflint, and GCP configuration into dedicated option structs and updates callers to use these instead of full TerragruntOptions.",
    "Configuration parsing is now constructed in a clearer and safer way with stronger defaulting. This introduces option-based ParsingContext construction with default TerraformCliArgs, adds nil-safety around CLI-arg usage, and expands external-facing tests for the config package.",
    "Filter and classification logic now performs less work at runtime and uses more explicit APIs. This eagerly compiles filter globs, removes logging from classifier matching, introduces builder-style GraphExpression and constructor-based Classifier, and reworks discovery/runners to consume parsed filter objects and explicit Terraform version detection results.",
    "Error matching for retries and ignores is now more predictable and easier to test. Matching is restricted to cleaned stderr plus the underlying error, message formatting now preserves '-' and '=', and matching helpers are exported with dedicated tests.",
    "GCP authentication behavior is now more correct when multiple credential mechanisms are configured. The Terraform/OpenTofu helper APIs now handle typed GCP credentials files and ensure GCP impersonation credentials correctly override base credentials.",
    "Hook failure messages now provide more of the information needed to diagnose a failure. The logging now includes the command, exit code, and output details, and tflint errors are wrapped more consistently with new tests and fixtures for hook error reporting.",
    "Provider version listings are now cleaner and less error-prone for consumers. The provider versions API now filters to only valid semver values and logs when invalid versions are skipped.",
    "Provider cache behavior is now verified for dependency output resolution to reduce regressions. This adds fixtures and an integration test that confirm provider cache usage when resolving dependency outputs.",
    "Complex environment variable values are now handled more safely when they include interpolation-like patterns. The implementation escapes ${...} patterns in complex TF_VAR values while preserving literal strings, and adds dedicated tests plus CI memory tuning.",
    "Unit output flushing and path resolution are now more consistent and easier to validate. This uses unit.Path() instead of absolute paths, simplifies FlushOutput, and adds a regression test/fixture for detailed exit codes when using terraform source and working-dir.",
    "IAM assume-role defaults are now maintained in a more focused location. The default duration and session-name helper were moved from the general options package into an iam package and all usages were updated.",
    "S3/GCS client and attribute-expression utilities are now simpler and remove unused plumbing. This introduces a dedicated type attribute expression helper, removes the logger parameter from the GCS client constructor, and deletes unused Terraform and logging-related code.",
    "Default directory and path canonicalization logic is now more consistent and less error-prone. This simplifies path and default directory handling, tightens path canonicalization, removes related errors, and adds a helper for default IAM session names."
  ],
  "New Features": [
    "The documentation site now supports the new docs.terragrunt.com domain with updated routing and redirects. This change prepares the Starlight docs and codebase for docs.terragrunt.com by introducing redirects, updating schemas, removing the legacy marketing UI, and adding a permanent redirect from /contact-tgs to terragrunt.com/contact-tgs.",
    "The compatibility documentation and endpoints are now easier to access using path-based URLs instead of query parameters. This change introduces path-based compatibility API endpoints and updates documentation and routing to use the new path format."
  ],
  "Required Actions": [
    "The compatibility links you use may need updating to match the new URL structure. If you have bookmarks or automation that calls compatibility endpoints via query parameters, switch to the new path-based endpoints described in the updated documentation."
  ],
  "Resolved Issues": [
    "Terragrunt now fails fast with a clear error when conflicting configuration files are present in the same directory. Specifically, it detects and errors when both terragrunt.hcl and terragrunt.stack.hcl exist, documents the constraint, and adjusts macOS lint CI cache handling.",
    "Discovery now more reliably reports offline scenarios when network access is blocked or misconfigured. Any *url.Error from discovery is treated as offline, and a test was added for blocked-registry mirrors.",
    "Included configuration tracking is now more reliable when paths are provided relatively. Included config paths are normalized to absolute paths before being tracked, and a git filter test was added to cover relative includes.",
    "Line-length lint failures are now avoided without broadly weakening linting. The lll linter is enabled selectively and long Go function signatures are wrapped to satisfy line-length checks.",
    "Undefined flag errors for run commands now provide more actionable guidance. Terragrunt adds special undefined-flag handling for `run` commands with a passthrough hint and includes tests to validate the behavior.",
    "Module variable parsing now correctly handles provider function syntax in HCL. This adds a regression test to ensure ModuleVariables supports provider function syntax and prevents reintroducing the issue."
  ],
  "⚙️ Process Updates": [
    "Long-running CI and lint checks should be more stable on different platforms and workflows. This includes CI cache key refinements for Go, lint configuration adjustments, and workflow updates to newer supported GitHub Actions versions."
  ],
  "🏎️ Performance Improvements": [
    "Worker pool failure handling now does less work while preserving correct aggregation. Error handling was simplified to use only MultiError and tests were tightened to verify the exact error count."
  ],
  "🐛 Bug Fixes": [
    "Worktree diff filter expansion now fails with a proper error instead of silently continuing. Filter globs are now compiled eagerly and worktree diff filter expansion returns errors, with all callers and tests updated for the new APIs and error handling.",
    "Terragrunt no longer mis-handles Terragrunt backend initialization when remote_state.disable_init is set. The runner now skips the Terragrunt backend bootstrap while still passing backend-config arguments to Terraform, with updated tests, fixtures, and docs to prevent regressions.",
    "Hook error reporting now consistently captures the right failure details for troubleshooting. The implementation enriches hook failure logs with command/exit-code/output details, improves tflint error wrapping, and adds tests/fixtures to validate the new reporting."
  ],
  "📖 Documentation Updates": [
    "Documentation now reflects current behavior for several deprecated or migrated CLI options. The strict controls and CLI migration docs were updated to account for deprecations and newly completed behaviors across multiple flags and options.",
    "The documentation site structure was reorganized to better support the current build and lint rules. The Starlight-based docs were moved into the `docs` directory and the configs, scripts, and markdown were updated to match the new location and lint expectations.",
    "Cross-repository references in docs and CI text now point to the correct upstream locations. This updates documentation links and CI descriptions to reference the current GitHub repositories and GitHub Actions.",
    "A deprecated guide page now reads more cleanly without redundant headings. This removes the 'Overview' heading from the deprecated attributes migration guide."
  ],
  "🛠️ Breaking Changes": [
    "Compatibility endpoints now use path-based URLs rather than query-parameter URLs. This introduces new path-based compatibility API endpoints and updates docs and routing, so clients that depended on the old query format may need to update their URLs.",
    "Directories containing both terragrunt.hcl and terragrunt.stack.hcl are now treated as invalid configurations. Terragrunt explicitly detects this condition, errors with a clear message, and documents the constraint so workflows must ensure only one of these files exists per directory."
  ],
  "🧹 Chores": [
    "Automation caching is now more precise to reduce accidental cache reuse across different environments. GitHub Actions Go-related cache restore keys are now more OS- and go.sum-specific to improve cache correctness.",
    "CI and build metadata discovery now avoids scanning non-source files. LINT_TAGS discovery in the Makefile is restricted to Go source files only.",
    "Internal package layout is now more modular to reduce tight coupling between components. Shared types and constants were moved out of pkg/options into focused internal packages and callers were updated, including retry defaults, Terraform implementation type selection, default dirs, and TF data dir usage.",
    "Test infrastructure now relies less on external helper libraries. Terratest helpers were replaced with internal utilities and related dependencies were cleaned up.",
    "Shell execution option construction is now standardized across the codebase. shell.RunOptionsFromOpts was replaced with new ShellOptions helper constructors and all callers were updated.",
    "Several third-party dependencies were updated to keep the project current and secure. This updates aws-sdk-go-v2 modules, bumps sops to v3.12.1, updates github.com/cloudflare/circl in test/flake, and refreshes additional Go module versions in go.mod and go.sum.",
    "Documentation build and workflow dependencies were refreshed to current versions. This updates docs-starlight JavaScript dependencies and lockfiles, bumps the AWS SDK v3 dependencies in the docs fixture app, and updates multiple GitHub Actions (setup-go, upload-artifact, download-artifact, stale, cosign-installer, DigiCert signing, and jdx/mise-action) by pinning to newer versions/SHAs.",
    "The web configuration was updated to reflect the current primary domain. CORS allowed origins were changed from terragrunt.webflow.io to terragrunt.com."
  ]
}
):

{
  "Improvements": [
    "Running with certain remote state settings now avoids unnecessary setup work while preserving correct backend behavior. Specifically, remote_state.disable_init now skips Terragrunt backend bootstrap while still passing backend-config arguments through to Terraform, and the runner logic, tests, fixtures, and documentation were updated accordingly.",
    "File and working-directory handling is now more consistent across platforms and environments. The implementation replaces many uses of filepath.Abs/ToSlash with root-working-dir–relative joins plus filepath.Clean, passes the root working directory into catalog repo creation instead of using os.Getwd, and updates discovery/stack/path utilities plus tests and integrations for the new normalization rules.",
    "Command output handling is now more consistent and less tightly coupled to global options. Output processing is centralized in a Writers struct and the engine/shell/Terraform/config parsing layers now use dedicated engine and run option structs rather than passing full TerragruntOptions.",
    "Configuration parsing and execution options are now more modular and easier to reason about. This refactors option passing by extracting provider-cache, backend, remote-state, run, tf, tflint, and GCP configuration into dedicated option structs and updates callers to use these instead of full TerragruntOptions.",
    "Configuration parsing is now constructed in a clearer and safer way with stronger defaulting. This introduces option-based ParsingContext construction with default TerraformCliArgs, adds nil-safety around CLI-arg usage, and expands external-facing tests for the config package.",
    "Filter and classification logic now performs less work at runtime and uses more explicit APIs. This eagerly compiles filter globs, removes logging from classifier matching, introduces builder-style GraphExpression and constructor-based Classifier, and reworks discovery/runners to consume parsed filter objects and explicit Terraform version detection results.",
    "Error matching for retries and ignores is now more predictable and easier to test. Matching is restricted to cleaned stderr plus the underlying error, message formatting now preserves '-' and '=', and matching helpers are exported with dedicated tests.",
    "GCP authentication behavior is now more correct when multiple credential mechanisms are configured. The Terraform/OpenTofu helper APIs now handle typed GCP credentials files and ensure GCP impersonation credentials correctly override base credentials.",
    "Hook failure messages now provide more of the information needed to diagnose a failure. The logging now includes the command, exit code, and output details, and tflint errors are wrapped more consistently with new tests and fixtures for hook error reporting.",
    "Provider version listings are now cleaner and less error-prone for consumers. The provider versions API now filters to only valid semver values and logs when invalid versions are skipped.",
    "Provider cache behavior is now verified for dependency output resolution to reduce regressions. This adds fixtures and an integration test that confirm provider cache usage when resolving dependency outputs.",
    "Complex environment variable values are now handled more safely when they include interpolation-like patterns. The implementation escapes ${...} patterns in complex TF_VAR values while preserving literal strings, and adds dedicated tests plus CI memory tuning.",
    "Unit output flushing and path resolution are now more consistent and easier to validate. This uses unit.Path() instead of absolute paths, simplifies FlushOutput, and adds a regression test/fixture for detailed exit codes when using terraform source and working-dir.",
    "IAM assume-role defaults are now maintained in a more focused location. The default duration and session-name helper were moved from the general options package into an iam package and all usages were updated.",
    "S3/GCS client and attribute-expression utilities are now simpler and remove unused plumbing. This introduces a dedicated type attribute expression helper, removes the logger parameter from the GCS client constructor, and deletes unused Terraform and logging-related code.",
    "Default directory and path canonicalization logic is now more consistent and less error-prone. This simplifies path and default directory handling, tightens path canonicalization, removes related errors, and adds a helper for default IAM session names."
  ],
  "New Features": [
    "The documentation site now supports the new docs.terragrunt.com domain with updated routing and redirects. This change prepares the Starlight docs and codebase for docs.terragrunt.com by introducing redirects, updating schemas, removing the legacy marketing UI, and adding a permanent redirect from /contact-tgs to terragrunt.com/contact-tgs.",
    "The compatibility documentation and endpoints are now easier to access using path-based URLs instead of query parameters. This change introduces path-based compatibility API endpoints and updates documentation and routing to use the new path format."
  ],
  "Required Actions": [
    "The compatibility links you use may need updating to match the new URL structure. If you have bookmarks or automation that calls compatibility endpoints via query parameters, switch to the new path-based endpoints described in the updated documentation."
  ],
  "Resolved Issues": [
    "Terragrunt now fails fast with a clear error when conflicting configuration files are present in the same directory. Specifically, it detects and errors when both terragrunt.hcl and terragrunt.stack.hcl exist, documents the constraint, and adjusts macOS lint CI cache handling.",
    "Discovery now more reliably reports offline scenarios when network access is blocked or misconfigured. Any *url.Error from discovery is treated as offline, and a test was added for blocked-registry mirrors.",
    "Included configuration tracking is now more reliable when paths are provided relatively. Included config paths are normalized to absolute paths before being tracked, and a git filter test was added to cover relative includes.",
    "Line-length lint failures are now avoided without broadly weakening linting. The lll linter is enabled selectively and long Go function signatures are wrapped to satisfy line-length checks.",
    "Undefined flag errors for run commands now provide more actionable guidance. Terragrunt adds special undefined-flag handling for `run` commands with a passthrough hint and includes tests to validate the behavior.",
    "Module variable parsing now correctly handles provider function syntax in HCL. This adds a regression test to ensure ModuleVariables supports provider function syntax and prevents reintroducing the issue."
  ],
  "⚙️ Process Updates": [
    "Long-running CI and lint checks should be more stable on different platforms and workflows. This includes CI cache key refinements for Go, lint configuration adjustments, and workflow updates to newer supported GitHub Actions versions."
  ],
  "🏎️ Performance Improvements": [
    "Worker pool failure handling now does less work while preserving correct aggregation. Error handling was simplified to use only MultiError and tests were tightened to verify the exact error count."
  ],
  "🐛 Bug Fixes": [
    "Worktree diff filter expansion now fails with a proper error instead of silently continuing. Filter globs are now compiled eagerly and worktree diff filter expansion returns errors, with all callers and tests updated for the new APIs and error handling.",
    "Terragrunt no longer mis-handles Terragrunt backend initialization when remote_state.disable_init is set. The runner now skips the Terragrunt backend bootstrap while still passing backend-config arguments to Terraform, with updated tests, fixtures, and docs to prevent regressions.",
    "Hook error reporting now consistently captures the right failure details for troubleshooting. The implementation enriches hook failure logs with command/exit-code/output details, improves tflint error wrapping, and adds tests/fixtures to validate the new reporting."
  ],
  "📖 Documentation Updates": [
    "Documentation now reflects current behavior for several deprecated or migrated CLI options. The strict controls and CLI migration docs were updated to account for deprecations and newly completed behaviors across multiple flags and options.",
    "The documentation site structure was reorganized to better support the current build and lint rules. The Starlight-based docs were moved into the `docs` directory and the configs, scripts, and markdown were updated to match the new location and lint expectations.",
    "Cross-repository references in docs and CI text now point to the correct upstream locations. This updates documentation links and CI descriptions to reference the current GitHub repositories and GitHub Actions.",
    "A deprecated guide page now reads more cleanly without redundant headings. This removes the 'Overview' heading from the deprecated attributes migration guide."
  ],
  "🛠️ Breaking Changes": [
    "Compatibility endpoints now use path-based URLs rather than query-parameter URLs. This introduces new path-based compatibility API endpoints and updates docs and routing, so clients that depended on the old query format may need to update their URLs.",
    "Directories containing both terragrunt.hcl and terragrunt.stack.hcl are now treated as invalid configurations. Terragrunt explicitly detects this condition, errors with a clear message, and documents the constraint so workflows must ensure only one of these files exists per directory."
  ],
  "🧹 Chores": [
    "Automation caching is now more precise to reduce accidental cache reuse across different environments. GitHub Actions Go-related cache restore keys are now more OS- and go.sum-specific to improve cache correctness.",
    "CI and build metadata discovery now avoids scanning non-source files. LINT_TAGS discovery in the Makefile is restricted to Go source files only.",
    "Internal package layout is now more modular to reduce tight coupling between components. Shared types and constants were moved out of pkg/options into focused internal packages and callers were updated, including retry defaults, Terraform implementation type selection, default dirs, and TF data dir usage.",
    "Test infrastructure now relies less on external helper libraries. Terratest helpers were replaced with internal utilities and related dependencies were cleaned up.",
    "Shell execution option construction is now standardized across the codebase. shell.RunOptionsFromOpts was replaced with new ShellOptions helper constructors and all callers were updated.",
    "Several third-party dependencies were updated to keep the project current and secure. This updates aws-sdk-go-v2 modules, bumps sops to v3.12.1, updates github.com/cloudflare/circl in test/flake, and refreshes additional Go module versions in go.mod and go.sum.",
    "Documentation build and workflow dependencies were refreshed to current versions. This updates docs-starlight JavaScript dependencies and lockfiles, bumps the AWS SDK v3 dependencies in the docs fixture app, and updates multiple GitHub Actions (setup-go, upload-artifact, download-artifact, stale, cosign-installer, DigiCert signing, and jdx/mise-action) by pinning to newer versions/SHAs.",
    "The web configuration was updated to reflect the current primary domain. CORS allowed origins were changed from terragrunt.webflow.io to terragrunt.com."
  ]
}

-->
