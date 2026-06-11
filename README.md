# YACD Pipeline Examples

Example pipeline YAML files for [YACD](https://github.com/r-devops/yacd) (Yet Another CD).

> **Full schema reference:** every YAML option supported by YACD is documented at
> [`docs/pipeline-reference.md`](https://github.com/r-devops/yacd/blob/main/docs/pipeline-reference.md) in the main repo.
> The examples here are recipes; the reference doc is the spec.

## Repo layout (invariant #147)

**All YACD configuration lives under the hidden `.yacd/` folder.** Anything outside
`.yacd/` is ignored by the pipeline scanner. This matches the industry pattern used
by GitHub Actions (`.github/workflows/`), CircleCI (`.circleci/`), and similar
tools.

```
.yacd/                                          ← ALL YACD config lives here
  build.yacd.yaml                               ← a pipeline
  team-a/
    deploy.yacd.yaml                            ← pipeline in a free-form subdir
  prod/
    nightly/
      rotate-password.yacd.yaml                 ← any depth is fine
  lib/                                          ← reserved: libraries only
    actions/
      upload-coverage/
        action.yacd.yaml                        ← consumed via `uses:`
    stages/
      go-build/
        stage.yacd.yaml                         ← consumed via `use:`
```

Key rules:

1. **`.yacd/**/*.yacd.yaml`** is the default pipeline scan path. Pipelines can sit at
   any depth under `.yacd/`, organised however you want (by team, env, service, etc).
2. **`.yacd/lib/`** is reserved for library files. Library files (`action.yacd.yaml`
   and `stage.yacd.yaml`) are loaded on demand when a pipeline references them via
   `uses:` or `use:`. They are NEVER imported as standalone pipelines.
3. **Anything outside `.yacd/`** is ignored entirely. Put your app code, docs,
   `lib/` directories from other tools, etc., anywhere you like — YACD will not touch
   them.

Inside a pipeline that consumes a library, the `uses:` path omits the `.yacd/`
prefix (the resolver fills it in):

```yaml
steps:
  - uses: yacd-examples@main/lib/actions/upload-coverage
    params:
      path: coverage.out
      service: codecov
```

## Materials (cloning additional repos per run)

The `materials:` block at the top of a pipeline tells the agent to clone
N extra repositories into named subdirs of the workspace before any
step runs. Three variants are demonstrated:

| File | What it shows |
|------|---------------|
| `.yacd/multi-material-demo.yacd.yaml` | **Multiple PUBLIC materials.** Two repos cloned side-by-side into `docs/` and `readme_src/`. No credentials. |
| `.yacd/private-material-demo.yacd.yaml.example` | **Single PRIVATE material with auth.** One repo, credential resolved by name from the Credentials store (encrypted at rest). |
| `.yacd/multi-material-with-auth.yacd.yaml.example` | **Mixed PUBLIC + PRIVATE materials with auth.** Both patterns side-by-side in one pipeline. |

The two `.example` files require an operator step (create the named credential
in the UI, then rename to drop the `.example` suffix); see the comment block
at the top of each file for the exact steps.

## Pipeline examples

| File | Description |
|------|-------------|
| `.yacd/build.yacd.yaml` | Build and test with configurable inputs (Go version, lint toggle) |
| `.yacd/deploy.yacd.yaml` | Deploy to target environment with input parameters |
| `.yacd/parallel-dag.yacd.yaml` | Fan-out/fan-in pattern with parallel stages |
| `.yacd/complex-dag.yacd.yaml` | Diamond dependency pattern with multiple convergence points |
| `.yacd/dag-leaf-stages.yacd.yaml` | Fan-out where one branch is a leaf (no downstream waits for it) |
| `.yacd/approval-gates.yacd.yaml` | Manual approval gates before production deployment |
| `.yacd/approval-restricted.yacd.yaml` | Approval gates with specific allowed users and groups |
| `.yacd/multi-env-deploy.yacd.yaml` | Progressive deployment: dev → staging → production with approvals |
| `.yacd/microservices.yacd.yaml` | Build and deploy multiple microservices with contract tests |
| `.yacd/conditional-stages.yacd.yaml` | Conditional stage execution with `when` expressions |
| `.yacd/finally-post.yacd.yaml` | `finally` stages and `post` hooks for guaranteed cleanup |
| `.yacd/retry-conditions.yacd.yaml` | Retry with backoff strategies and exit code filtering |
| `.yacd/plugin-example.yacd.yaml` | Plugin system with notifications and artifact upload |
| `.yacd/triggers-schedule.yacd.yaml` | Push, PR, and cron schedule triggers |
| `.yacd/manual-trigger.yacd.yaml` | Manual-only pipeline with rich input parameters |
| `.yacd/variables-env.yacd.yaml` | Pipeline variables and per-step environment overrides |
| `.yacd/custom-containers.yacd.yaml` | Multi-language builds (Go, Node, Python) |
| `.yacd/timeout-demo.yacd.yaml` | Pipeline and step-level timeout configuration |
| `.yacd/infra.yacd.yaml` | Infrastructure provisioning with Terraform |
| `.yacd/shared-libs-demo.yacd.yaml` | Demonstrates `uses:` and `use:` referencing `.yacd/lib/` |
| `.yacd/pipeline.yaml` | Large demo pipeline showcasing all features |

## Library examples

Library files live under `.yacd/lib/` and are consumed by pipelines via the
`uses:` / `use:` keywords. They are never triggered directly — invariant #147.

| Path | What it is |
|------|------------|
| `.yacd/lib/actions/upload-coverage/action.yacd.yaml` | Reusable step action with typed `inputs:` and `outputs:` |
| `.yacd/lib/stages/go-build/stage.yacd.yaml` | Reusable stage template |

## Usage

Add this repository as a pipeline group in YACD. The default scan path
`.yacd/**/*.yacd.yaml` picks up every pipeline shown above. Library files under
`.yacd/lib/` are excluded from the pipeline scan automatically.
