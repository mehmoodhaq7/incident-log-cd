# incident-log-cd

Kubernetes manifests for the `incident-log` application — managed by ArgoCD as the GitOps source of truth.

> This repo is not meant to be edited manually. Image tags are updated automatically by the Jenkins CI pipeline on every successful build.

---

## How It Works

```
Jenkins (incident-log)
    │
    ├── builds Docker images → pushes to DockerHub with :BUILD_NUMBER tag
    │
    └── clones this repo → updates image tags in backend.yaml + frontend.yaml
              │
              ▼ git push
    ArgoCD detects change via webhook
              │
              ▼ auto-sync + self-heal
    EKS prod namespace updated
```

## Structure

```
k8s-prod/
├── namespace.yaml      prod namespace
├── backend.yaml        Deployment + Service + ConfigMap + Secret
├── frontend.yaml       Deployment + Service
├── mysql.yaml          StatefulSet + headless Service + ConfigMap + Secret
└── ingress.yaml        NGINX ingress + TLS (cert-manager)
```

## Related

- **[incident-log](https://github.com/mehmoodhaq7/incident-log)** — Application source code + Jenkins CI pipeline
