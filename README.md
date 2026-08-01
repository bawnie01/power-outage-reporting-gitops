# Power Outage Reporting GitOps

This repository stores the desired Kubernetes state for the Power Outage
Reporting System. Argo CD monitors this repository and synchronizes the `dev`
environment to Kubernetes.

## Repository Structure

```text
power-outage-reporting-gitops/
├── bootstrap/
│   └── power-outage-dev.yaml
└── environments/
    └── dev/
        └── kustomization.yaml
```

## Deployment Flow

```text
Application source repository
        |
        | test, build and publish images
        v
GitHub Container Registry
        |
        | immutable image tags
        v
GitOps repository
        |
        | watched by Argo CD
        v
Kubernetes cluster
```

## Current Release

The `dev` environment uses container tag `sha-775e5a1`. Kubernetes manifests
are pinned separately to a full source commit, allowing deployment corrections
without rebuilding unchanged application images.

## Preview

```powershell
kubectl kustomize environments/dev
```

## Bootstrap Argo CD Application

Verify the current Kubernetes context before applying anything:

```powershell
kubectl config current-context
kubectl get nodes
```

Use only a disposable learning cluster. Then apply the root Application:

```powershell
kubectl apply -f bootstrap/power-outage-dev.yaml
```

Argo CD will create the `power-outage` namespace and synchronize the complete
platform. The source CI pipeline does not require Kubernetes credentials.
