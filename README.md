# K8s Manifest

### AVP note:
- https://github.com/luafanti/arogcd-vault-plugin-example
- create Argo CD app with demo k8s resources 
```
argocd app create argocd-vault-plugin-demo --repo https://github.com/luafanti/arogcd-vault-plugin-example.git --path kubernetes-resources --dest-namespace argocd-vault-demo --dest-server https://kubernetes.default.svc --config-management-plugin argocd-vault-plugin
```
- Argocd application with avp plugin
```
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: python-api-app-development
  namespace: argocd
spec:
  project: development
  source:
    repoURL: git@gitlab.com:kienlt-cicd/k8s-manifest.git
    targetRevision: main
    path: python-api-app
    plugin:
      name: argocd-vault-plugin-helm
      env:
        - name: HELM_ARGS
          value: -f dev.yaml
  destination:
    server: https://kubernetes.default.svc
    namespace: python-app
  syncPolicy:
    syncOptions:
      - CreateNamespace=true
```