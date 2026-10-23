# Traefik installation runbook

This runbook records the commands, decisions, validation, and rollback for the Traefik profile. Commands are run from the Project 03 repository unless stated otherwise.

## Step 1 - chart discovery and version pinning

### Objective

Add the official Traefik chart repository locally and select a reproducible chart version before any cluster change.

### Commands executed

```powershell
helm repo add traefik https://traefik.github.io/charts
helm repo update

helm search repo traefik/traefik --versions |
  Select-Object -First 8
```

### Result

The newest version returned was chart `42.1.3`, which packages Traefik Proxy `v3.7.16`. The exact versions are committed in [`controllers/traefik/helm/chart-lock.yaml`](../controllers/traefik/helm/chart-lock.yaml).

### Decision

Use `traefik/traefik` version `42.1.3` for this project. The Helm install command must include `--version 42.1.3`; it must not use an implicit latest version.

### Cluster impact

None. These commands only changed the local Helm repository cache.

## Next step

The Ubuntu WSL2 workstation was prepared and verified before continuing. Next, retrieve the chart defaults into `controllers/traefik/helm/values.yaml`, then review and modify the values that request a dedicated Cilium LoadBalancer IP.
