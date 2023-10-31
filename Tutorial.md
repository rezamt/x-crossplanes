# upbound/provider-azuread

## Installing UP Cli
``` bash
# Install upbound cli - https://github.com/upbound/up
brew install upbound/tap/up

# Check Installation : run up
# up
up
Usage: up <command>

The Upbound CLI

Flags:
  -h, --help                Show context-sensitive help.
      --format="default"    Format for get/list commands. Can be: json, yaml, default
  -v, --version             Print version and exit.
  -q, --quiet               Suppress all output.
      --pretty              Pretty print output.

Commands:
  license                Print Up license information.
  help                   Show help.
  login                  Login to Upbound.
  logout                 Logout of Upbound.
  configuration (cfg)    Interact with configurations.
  controlplane (ctp)     Interact with control planes.
  organization (org)     Interact with organizations.
  profile                Interact with Upbound profiles.
  repository (repo)      Interact with repositories.
  robot                  Interact with robots.
  uxp                    Interact with UXP.
  xpkg                   Interact with UXP packages.
  xpls                   Start xpls language server.
  alpha                  Alpha features. Commands may be removed in future releases.
  install-completions    Install shell completions
  space                  Interact with spaces.

Run "up <command> --help" for more information on a command.

```

## Running a K8s local cluster (using Kind)
```
# Using Kind
brew install kind

# One Master + 2 Worker Config
cat > kind-config.yaml <<EOF
# three node (two workers) cluster config
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
- role: worker
EOF

# Creat the Cluster
kind create cluster --name x-crossplane --config kind-config.yaml

# Installing Metal-lb LB on Kind


```
## Installing upbound/provider-azuread

```bash

# Install Universal Crossplane
up uxp install

# check the state
kubectl get pods -n upbound-system

NAME                                       READY   STATUS    RESTARTS   AGE
crossplane-779968bb44-2v5sr                1/1     Running   0          42s
crossplane-rbac-manager-6f8d84b588-t5ggs   1/1     Running   0          42s


# Install the official Azuread provider
# cat > az-provider.yaml <<EOF
apiVersion: pkg.crossplane.io/v1
kind: Provider
metadata:
  name: provider-azuread
spec:
  package: xpkg.upbound.io/upbound/provider-azuread:v0.13.0
EOF

# kubectl apply -f az-provider.yaml

# kubectl get provider
NAME             INSTALLED   HEALTHY   PACKAGE                                         AGE
provider-azuread   True        True      xpkg.upbound.io/upbound/provider-azuread:v0.1.0   58s




```


# References
- [Upbound Cli](https://github.com/upbound/up)
- [Kind Cluster](https://mcvidanagama.medium.com/set-up-a-multi-node-kubernetes-cluster-locally-using-kind-eafd46dd63e5)
- [upbound / provider-azuread](https://github.com/upbound/provider-azuread)
- [upbound / provider-azuread quickstart](https://marketplace.upbound.io/providers/upbound/provider-azure/v0.38.1/docs/quickstart)
- 
