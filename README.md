# devops-toolkit

A small, working reference for a modern CI/CD and Kubernetes delivery setup.
Generic example material only.

## What's inside

| Path | Purpose |
|---|---|
| `app/main.py` | Tiny dependency-free HTTP service with `/healthz` and `/readyz` |
| `docker/Dockerfile` | Multi-stage, non-root, health-checked container image |
| `.github/workflows/ci.yml` | GitHub Actions: lint, test, build image, scan |
| `azure-pipelines.yml` | The same flow as an Azure DevOps multi-stage pipeline |
| `k8s/` | Deployment, Service, HPA and PodDisruptionBudget with sane defaults |

## Production-readiness defaults in `k8s/`

- Resource requests and limits on every container
- Readiness, liveness and startup probes
- Non-root user, read-only root filesystem, dropped capabilities
- Rolling update with `maxUnavailable: 0`
- HPA on CPU and a PodDisruptionBudget so node drains don't cause outages
- Pod anti-affinity spread across nodes

## Pipeline flow

```
commit -> lint + unit tests -> build image -> vulnerability scan -> push -> deploy to non-prod -> approval -> prod
```

## Try it locally

```bash
docker build -f docker/Dockerfile -t demo-app .
docker run --rm -p 8080:8080 demo-app
curl localhost:8080/healthz
```

## License

MIT
