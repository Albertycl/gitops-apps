# gitops-apps

GitOps repo for my demo apps. Argo CD on a local K3s cluster watches this repo and deploys what is in it.

## How it works

1. A push to an app repo runs GitHub Actions: test, build, push the image to GHCR.
2. The workflow commits the new image tag into `manifests/<app>/` in this repo.
3. Argo CD sees the commit and syncs the cluster.

## Layout

```
argocd/root-app.yaml   app-of-apps: the one Application applied by hand
apps/                  one Argo CD Application per app
manifests/<app>/       Kubernetes YAML for that app
```

## Rules

- No secrets in this repo. Secrets are created in the cluster separately.
- To add an app: put its YAML in `manifests/<app>/` and add `apps/<app>.yaml`.

## Bootstrap

```
kubectl apply -f argocd/root-app.yaml
```
