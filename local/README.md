# Sample app - local

A minimal Deployment + Service to verify a GitOps repo (from
`templates/gitops`, `deployment_target: local`) end to end. Not generated
by any template - drop `app.yaml` into `apps/<name>/` in your generated
GitOps repo and push.

No Ingress here - `deployment_target: local` has no ALB and no real DNS.
ArgoCD picks this up automatically since the root Application's directory
source recurses over the whole repo and applies any plain manifest it
finds (same as `apps/cluster-addons/storage-class.yaml`) - no separate
`Application` needed.

## Testing steps

```bash
kubectl port-forward -n default svc/app 8081:80
curl http://localhost:8081
```

Expect `hello from the sample app`. If the pod never comes up, check
`kubectl get application -n argocd` (confirm the root Application actually
synced) before assuming the manifest is wrong.
