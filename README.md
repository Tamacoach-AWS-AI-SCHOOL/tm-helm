# tm-helm

Helm chart template repository for `tm-app`.

This repository only contains reusable chart templates.
Environment-specific values and Kubernetes resources (for example `dev`, `prod`, IRSA, ExternalSecret, TargetGroupBinding) must be managed in a separate manifest/config repository.

## Structure

```text
charts/
  tm-app/
    Chart.yaml
    values.yaml
    templates/
```

## Values Contract

The chart expects these keys in values files.

### Image

- `image.repository`: container image repository
- `image.digest`: immutable image digest (used as `repository@digest`)

### Workloads

- `workloads.backend.enabled`: enable/disable backend deployment
- `workloads.backend.replicas`: backend replica count (preferred)
- `workloads.backend.replicaCount`: backend replica count (legacy-compatible)
- `workloads.backend.command`: backend container command array
- `workloads.backend.args`: backend container args array
- `workloads.backend.envFrom.secretName`: backend secret name for `envFrom`

- `workloads.worker.enabled`: enable/disable worker deployment
- `workloads.worker.replicas`: worker replica count (preferred)
- `workloads.worker.replicaCount`: worker replica count (legacy-compatible)
- `workloads.worker.command`: worker container command array
- `workloads.worker.args`: worker container args array
- `workloads.worker.envFrom.secretName`: worker secret name for `envFrom`

Replica resolution in templates:
- `replicas` is used first
- fallback to `replicaCount`
- default is `1`

## Lint

Run:

```bash
helm lint charts/tm-app
```

## CI Cross-Repo Clone Prerequisites

`tm-helm` pipeline clones private `tm-manifest` repository with `CI_JOB_TOKEN`.

Configure permissions in target repository (`tm-manifest`):

1. Go to `Settings > CI/CD > Job token permissions`
2. Enable inbound CI job token access
3. Add source project (`tm-helm`) to allowlist
4. Ensure `read_repository` scope is allowed

Clone URL pattern used by CI:

```text
https://gitlab-ci-token:${CI_JOB_TOKEN}@gitlab.tamacoach.net/tamacoach/tm-manifest.git
```
