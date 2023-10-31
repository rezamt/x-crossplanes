
kind-cluster
```yaml
# three node (two workers) cluster config
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
- role: worker
```

az-provider.yaml

```yaml
apiVersion: pkg.crossplane.io/v1
kind: Provider
metadata:
  name: provider-azuread
spec:
  package: xpkg.upbound.io/upbound/provider-azuread:v0.13.0
  
```

az-provider-config.yaml
```yaml
apiVersion: azuread.upbound.io/v1beta1
metadata:
  name: default
kind: ProviderConfig
spec:
  credentials:
    source: Secret
    secretRef:
      namespace: upbound-system
      name: azuread-secret
      key: creds
```