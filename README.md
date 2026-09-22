# Kubernetes Deployments with Kustomize — Base + Overlays Example

A working `base` / `overlays` Kustomize project showing how to manage the same Kubernetes application across dev and prod without a templating language.

> 📖 Full write-up: [Simplify Kubernetes Deployments with Kustomize: A Guide to Efficient Configuration Management](https://www.linkedin.com/pulse/simplify-kubernetes-deployments-kustomize-guide-efficient-khatri-eefac/) by **Gaurav Khatri**

## Why Kustomize

- **Native integration** — built into `kubectl`, nothing extra to install
- **No templating language** — you work directly with standard YAML
- **Declarative** — you declare *how* manifests should be customized, not a script that generates them
- **Environment management** — dev/staging/prod configs without duplicating whole manifests

## How it works: bases and overlays

- **Base** — the resources common across every environment (here: a `Deployment` and a `Service`)
- **Overlay** — patches applied on top of a base to customize it for one environment (here: replica count)

## Repository layout

```
.
├── base/
│   ├── deployment.yaml
│   ├── service.yaml
│   └── kustomization.yaml
└── overlays/
    ├── dev/
    │   ├── kustomization.yaml
    │   └── replica-patch.yaml
    └── prod/
        ├── kustomization.yaml
        └── replica-patch.yaml
```

`base/kustomization.yaml` lists the shared resources. Each overlay's `kustomization.yaml` points back at `../../base` and applies a strategic-merge patch — in this example just `replicas` (1 in dev, 5 in prod), but the same pattern extends to image tags, resource limits, env vars, or anything else that differs per environment.

## Try it

```bash
# Render (without applying) to see exactly what each overlay produces
kubectl kustomize ./overlays/dev
kubectl kustomize ./overlays/prod

# Apply directly
kubectl apply -k ./overlays/dev
kubectl apply -k ./overlays/prod
```

## Takeaway

Kustomize handles real customization complexity — different replica counts, environment-specific config, staged rollouts — without introducing a templating language, which keeps the manifests themselves easy to read and onboard new team members onto.

---

**Author:** [Gaurav Khatri](https://www.linkedin.com/in/gaurav-khatri-devops/) — DevOps Engineer @ Sarv.com | Kubernetes (EKS), Docker, GitOps & CI/CD
