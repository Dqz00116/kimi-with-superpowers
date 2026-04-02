# Workflow Configuration Guide

> **How to adapt the Universal Workflow to your technology stack**

---

## Quick Setup

1. Copy the template below to your project root as `PROJECT_CONFIG.yaml`
2. Fill in your stack-specific values
3. AI assistants will automatically load it

---

## Configuration Template

```yaml
---
# Universal Workflow Configuration
name: "your-project-name"
extends: "universal-workflow-v1.0"
---

# =============================================================================
# TECHNOLOGY STACK
# =============================================================================

stack:
  language: python          # python | typescript | go | rust | java | csharp | ...
  language_version: "3.11"  # specific version
  framework: fastapi        # django | fastapi | flask | react | gin | ...
  framework_version: "0.104"
  
# =============================================================================
# COMMANDS (These are used in the workflow stages)
# =============================================================================

commands:
  # Testing
  test: "pytest"
  test_unit: "pytest tests/unit -v"
  test_integration: "pytest tests/integration -v"
  test_all: "pytest"
  test_watch: "pytest -f"
  
  # Building
  build: "poetry build"
  build_dev: "echo 'No build step for dev'"
  
  # Linting & Quality
  lint: "ruff check ."
  lint_fix: "ruff check . --fix"
  format: "ruff format ."
  format_check: "ruff format --check ."
  type_check: "mypy src/"
  
  # Security
  security_scan: "bandit -r src/"
  
  # Regression
  regression_test: "./scripts/regression-test.sh"
  
  # Version Control
  vcs_diff: "git diff HEAD~1"
  get_base_sha: "git rev-parse HEAD~1"
  get_head_sha: "git rev-parse HEAD"

# =============================================================================
# PATHS (Where workflow artifacts are stored)
# =============================================================================

paths:
  # Workflow outputs
  design_docs: "docs/designs/"
  plans: "docs/plans/"
  
  # Code locations
  source: "src/"
  tests:
    root: "tests/"
    unit: "tests/unit/"
    integration: "tests/integration/"
    e2e: "tests/e2e/"
  
  # Documentation
  docs: "docs/"
  
  # Scripts
  scripts: "scripts/"

# =============================================================================
# CONSTRAINTS (Quality standards)
# =============================================================================

constraints:
  - "ZERO warnings (enforced)"
  - "ZERO mocks (use real instances)"
  - "100% type coverage"
  - "All tests must pass before merge"

# =============================================================================
# STAGE CUSTOMIZATIONS (Optional per-stage overrides)
# =============================================================================

stage_customizations:
  # Stage 5: Testing - Which commands to run
  stage_5:
    test_sequence:
      - command: "{test_unit}"
        description: "Unit tests"
      - command: "{test_integration}"
        description: "Integration tests"
      - command: "{regression_test}"
        description: "Regression tests"
  
  # Stage 6: Verification - What proves completion
  stage_6:
    verification_commands:
      - command: "{test_all}"
        expected: "0 failures, exit code 0"
      - command: "{lint}"
        expected: "0 errors"

# =============================================================================
# NAMING CONVENTIONS
# =============================================================================

conventions:
  branches: "feature/ISSUE-123-short-description"
  commits: "[TYPE]: Description (fixes #123)"
  
  files:
    style: "snake_case"      # snake_case | kebab-case | PascalCase | camelCase
    test_prefix: "test_"
    test_suffix: ""

# =============================================================================
# SKILL EXTENSIONS (Optional additions to Superpowers skills)
# =============================================================================

skill_extensions:
  brainstorming:
    additional_considerations:
      - "Check for breaking API changes"
      - "Consider database migration needs"
      - "Evaluate async/sync implications"
  
  writing-plans:
    required_sections:
      - "Migration steps"
      - "Rollback plan"
  
  verification_before_completion:
    additional_checks:
      - "Run security scan"
      - "Check test coverage"

# =============================================================================
# CUSTOM STAGES (Optional additional stages)
# =============================================================================

# custom_stages:
#   after_stage_3:
#     - name: "API Contract Check"
#       command: "openapi-diff baseline.yaml current.yaml"
#       condition: "files_changed include 'api/'"
#   
#   after_stage_5:
#     - name: "Performance Benchmark"
#       command: "pytest tests/benchmark/"
#       condition: "always"
```

---

## Examples by Technology

### Python (FastAPI + pytest)

```yaml
stack:
  language: python
  language_version: "3.11"
  framework: fastapi
  
commands:
  test: "pytest"
  test_unit: "pytest tests/unit -v"
  test_integration: "pytest tests/integration -v"
  build: "poetry build"
  lint: "ruff check ."
  format: "ruff format ."
  type_check: "mypy src/"

paths:
  design_docs: "docs/designs/"
  plans: "docs/plans/"
  source: "src/"
  tests:
    unit: "tests/unit/"
    integration: "tests/integration/"

constraints:
  - "ZERO warnings"
  - "100% type coverage with mypy"
```

### TypeScript (Node + React + Vitest)

```yaml
stack:
  language: typescript
  framework: react
  
commands:
  test: "npm run test"
  test_unit: "npm run test:unit"
  test_integration: "npm run test:integration"
  build: "npm run build"
  lint: "npm run lint"
  type_check: "npm run type-check"

paths:
  design_docs: "docs/designs/"
  plans: "docs/plans/"
  source: "src/"
  tests:
    unit: "src/**/*.test.ts"
    integration: "tests/integration/"

constraints:
  - "ZERO TypeScript errors"
  - "ZERO ESLint warnings"
```

### Go

```yaml
stack:
  language: go
  language_version: "1.21"
  
commands:
  test: "go test ./..."
  test_unit: "go test ./internal/... -v"
  test_integration: "go test ./tests/integration/... -v"
  build: "go build ./..."
  lint: "golangci-lint run"
  format: "gofmt -l ."

paths:
  design_docs: "docs/designs/"
  plans: "docs/plans/"
  source: "."
  tests:
    unit: "./internal/..."
    integration: "./tests/integration/..."

constraints:
  - "ZERO warnings"
  - "gofmt compliant"
  - "go vet clean"
```

### Rust

```yaml
stack:
  language: rust
  
commands:
  test: "cargo test"
  test_unit: "cargo test --lib"
  test_integration: "cargo test --test integration"
  build: "cargo build --release"
  lint: "cargo clippy -- -D warnings"
  format: "cargo fmt --check"

paths:
  design_docs: "docs/designs/"
  plans: "docs/plans/"
  source: "src/"
  tests:
    unit: "src/"
    integration: "tests/"

constraints:
  - "ZERO clippy warnings"
  - "cargo fmt compliant"
```

---

## Variable Substitution

In the workflow, these variables are replaced with your config values:

| Variable | Example Value |
|----------|---------------|
| `{test_command}` | `pytest` |
| `{test_unit}` | `pytest tests/unit -v` |
| `{test_integration}` | `pytest tests/integration -v` |
| `{build_command}` | `poetry build` |
| `{lint_command}` | `ruff check .` |
| `{design_docs_path}` | `docs/designs/` |
| `{plans_path}` | `docs/plans/` |
| `{unit_test_path}` | `tests/unit/` |
| `{integration_test_path}` | `tests/integration/` |
| `{vcs_diff_command}` | `git diff HEAD~1` |

---

## Validation

Your configuration should:

- [ ] Define all required commands (test, build, lint)
- [ ] Define all paths
- [ ] List project constraints
- [ ] Be valid YAML syntax
- [ ] Use existing commands (test them!)

---

## Loading the Configuration

AI assistants will automatically look for and load `PROJECT_CONFIG.yaml` from the project root before starting any task.

If not found, the workflow falls back to universal defaults with placeholders.

---

*Configuration Guide v1.0 | Universal Superpowers Workflow*
