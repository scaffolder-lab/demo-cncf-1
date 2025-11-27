# Architecture

## Deployment Flow

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│  Backstage  │────▶│   GitHub    │────▶│   ghcr.io   │────▶│   ArgoCD    │
│  Scaffolder │     │   Actions   │     │  Registry   │     │    Sync     │
└─────────────┘     └─────────────┘     └─────────────┘     └─────────────┘
                                                                   │
                                                                   ▼
                                                            ┌─────────────┐
                                                            │ Kubernetes  │
                                                            │   Cluster   │
                                                            └─────────────┘
```

## Components

### GitHub Actions Workflow

The CI/CD pipeline automatically:
1. Builds Docker image from Dockerfile
2. Pushes to GitHub Container Registry (ghcr.io)
3. Updates K8s deployment manifest with new image tag
4. ArgoCD detects change and syncs

### Kubernetes Resources

- **Deployment**: Runs the nginx container with the landing page
- **Service**: ClusterIP service exposing port 80

### Monitoring

- **Backstage**: Shows component status, ArgoCD sync state, and K8s resources
- **ArgoCD**: GitOps continuous deployment with auto-sync enabled
