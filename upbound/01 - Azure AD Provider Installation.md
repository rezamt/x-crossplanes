![[Pasted image 20231101011321.png]]
### 01 - Installing UP CLI
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

### 02- Running a K8s local cluster (using Kind)

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
### 03 - Installing Provider (Azure AD)

```bash

# Install Universal Crossplane
# up uxp install

# check the state
# kubectl get pods -n upbound-system

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

## 04 - Create an Entra ID service principal

`You need to have a valid AZ Subscription`

```bash


az login --use-device-code

# your subscription id should replace the following one
export SID=SUBSCRIPTION_ID

az ad sp create-for-rbac --sdk-auth --role Owner --scopes /subscriptions/$SID

```

you should have an output like:

```json
{
  "clientId": "5d73973c-1933-4621-9f6a-9642db949768",
  "clientSecret": "24O8Q~db2DFJ123MBpB25hdESvV3Zy8bfeGYGcSd",
  "subscriptionId": "c02e2b27-21ef-48e3-96b9-a91305e9e010",
  "tenantId": "7060afec-1db7-4b6f-a44f-82c9c6d8762a",
  "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
  "resourceManagerEndpointUrl": "https://management.azure.com/",
  "activeDirectoryGraphResourceId": "https://graph.windows.net/",
  "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
  "galleryEndpointUrl": "https://gallery.azure.com/",
  "managementEndpointUrl": "https://management.core.windows.net/"
}
```

Save the result into : `azuread-credentials.json`


## 05 - Creating K8s Secret

Use `kubectl create secret -n upbound-system` to generate the Kubernetes secret object inside the Kubernetes cluster.

`kubectl create secret generic azuread-secret -n upbound-system --from-file=creds=./azuread-credentials.json`

View the secret with `kubectl describe secret`

```shell
$ kubectl describe secret azuread-secret -n upbound-system
Name:         azuread-secret
Namespace:    upbound-system
Labels:       <none>
Annotations:  <none>

Type:  Opaque

Data
====
creds:  629 bytes
```

## 06 - Create a ProviderConfig

Create a `ProviderConfig` Kubernetes configuration file to attach the Azuread credentials to the installed official provider.

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

Apply this configuration with `kubectl apply -f`.

**Note:** the `Providerconfig` value `spec.secretRef.name` must match the `name` of the secret in `kubectl get secrets -n upbound-system` and `spec.secretRef.key` must match the value in the `Data` section of the secret.

Verify the `ProviderConfig` with `kubectl describe providerconfigs`.

```yaml
$ kubectl describe providerconfigs
Name:         default
Namespace:
API Version:  azuread.upbound.io/v1beta1
Kind:         ProviderConfig
# Output truncated
Spec:
  Credentials:
    Secret Ref:
      Key:        creds
      Name:       azuread-secret
      Namespace:  upbound-system
    Source:       Secret
```

## 07 - Create a managed resource
Create a managed resource to verify the provider is functioning.

This example creates an Azuread resource group.

```yaml
apiVersion: applications.azuread.upbound.io/v1beta1
kind: Application
metadata:
  name: example
spec:
  forProvider:
    displayName: example
  providerConfigRef:
    name: default
```

**Note:** the `spec.providerConfigRef.name` must match the `ProviderConfig` `metadata.name` value.?????

Apply this configuration with `kubectl apply -f`.

Use `kubectl get managed` to verify resource group creation.

```shell
$ kubectl get managed
NAME                                                 READY   SYNCED   EXTERNAL-NAME                          AGE
application.application.azuread.upbound.io/example   True    True     d0a4a20a-b3f8-4629-b6ed-7951d750f35d   18m
```

Provider created the resource group when the values `READY` and `SYNCED` are `True`.

_Note:_ commands querying Azuread resources may be slow to respond because of Azuread API response times.

If the `READY` or `SYNCED` are blank or `False` use `kubectl describe` to understand why.

## 08 - Clean up managed resource

````
## Delete the managed resource
Remove the managed resource by using `kubectl delete -f` with the same `Application` object file. It takes a up to 5 minutes for Kubernetes to delete the resource and complete the command.

Verify removal of the resource group with `kubectl get application`

```shell
$ kubectl get application
No resources found
````

# References
- [Upbound Cli](https://github.com/upbound/up)
- [Kind Cluster](https://mcvidanagama.medium.com/set-up-a-multi-node-kubernetes-cluster-locally-using-kind-eafd46dd63e5)
- [upbound / provider-azuread](https://github.com/upbound/provider-azuread)
- [upbound / provider-azuread quickstart](https://marketplace.upbound.io/providers/upbound/provider-azure/v0.38.1/docs/quickstart)
- 
