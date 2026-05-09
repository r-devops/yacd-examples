# YACD Pipeline Examples

Example pipeline YAML files for [YACD](https://github.com/r-devops/yacd) (Yet Another CD).

## Examples

| File | Description |
|------|-------------|
| `build.yacd.yaml` | Build and test with configurable inputs (Go version, lint toggle) |
| `deploy.yacd.yaml` | Deploy to target environment with input parameters |
| `parallel-dag.yacd.yaml` | Fan-out/fan-in pattern with parallel stages |
| `complex-dag.yacd.yaml` | Diamond dependency pattern with multiple convergence points |
| `approval-gates.yacd.yaml` | Manual approval gates before production deployment |
| `approval-restricted.yacd.yaml` | Approval gates with specific allowed users and groups |
| `multi-env-deploy.yacd.yaml` | Progressive deployment: dev → staging → production with approvals |
| `microservices.yacd.yaml` | Build and deploy multiple microservices with contract tests |
| `conditional-stages.yacd.yaml` | Conditional stage execution with `when` expressions |
| `finally-post.yacd.yaml` | `finally` stages and `post` hooks for guaranteed cleanup |
| `retry-conditions.yacd.yaml` | Retry with backoff strategies and exit code filtering |
| `plugin-example.yacd.yaml` | Plugin system with notifications and artifact upload |
| `triggers-schedule.yacd.yaml` | Push, PR, and cron schedule triggers |
| `manual-trigger.yacd.yaml` | Manual-only pipeline with rich input parameters |
| `variables-env.yacd.yaml` | Pipeline variables and per-step environment overrides |
| `custom-containers.yacd.yaml` | Multi-language builds (Go, Node, Python) |
| `timeout-demo.yacd.yaml` | Pipeline and step-level timeout configuration |
| `infra.yacd.yaml` | Infrastructure provisioning with Terraform |
| `pipeline.yaml` | Large demo pipeline showcasing all features |

## Usage

Add this repository as a pipeline group in YACD to automatically sync all examples.
