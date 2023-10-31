# upbound/provider-azuread

## Installation
``` bash
# Install upbound cli
# https://github.com/upbound/up
brew install upbound/tap/up

# Check
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

## Run a K8s local
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


```
## Installing upbound/provider-azuread



# References
- [Upbound Cli](https://github.com/upbound/up)
- [Kind Cluster](https://mcvidanagama.medium.com/set-up-a-multi-node-kubernetes-cluster-locally-using-kind-eafd46dd63e5)
- [upbound / provider-azuread](https://marketplace.upbound.io/providers/upbound/provider-azure/v0.38.1/docs/quickstart)
