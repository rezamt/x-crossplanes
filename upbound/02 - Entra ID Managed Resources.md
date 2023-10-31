Managed Resources (MR) are Crossplane's representation of a resource in a cloud provider. Managed Resources are opinionated, Crossplane Resource Model (XRM) compliant Kubernetes Custom Resources that are installed by the provider.

https://marketplace.upbound.io/providers/upbound/provider-azuread/v0.13.0/managed-resources



![[Pasted image 20231101011756.png]]


## Application MR



![[Pasted image 20231101011629.png]]

example

```yaml
apiVersion: applications.azuread.upbound.io/v1beta1
kind: Application
metadata:
  annotations:
    meta.upbound.io/example-id: app/v1beta1/roleassignment
  labels:
    testing.upbound.io/example-name: example
  name: example
spec:
  forProvider:
    displayName: example
    requiredResourceAccess:
      - resourceAccess:
          - id: df021288-bdef-4463-88db-98f22de89214
            type: Role
          - id: b4e74841-8e56-480b-be8b-910348b18b4c
            type: Scope
        resourceAppId: 00000003-0000-0000-c000-000000000000
```

example-authorizing-app

```yaml
apiVersion: applications.azuread.upbound.io/v1beta1
kind: Application
metadata:
  annotations:
    meta.upbound.io/example-id: applications/v1beta1/preauthorized
  labels:
    testing.upbound.io/example-name: example-authorizing-app
  name: example-authorizing-app
spec:
  forProvider:
    api:
      - oauth2PermissionScope:
          - adminConsentDescription: Administer the application
            adminConsentDisplayName: Administer
            enabled: true
            id: ced9c4c3-c273-4f0f-ac71-a20377b90f9c
            type: Admin
            value: administer
          - adminConsentDescription: Access the application
            adminConsentDisplayName: Access
            enabled: true
            id: 2d5e07ca-664d-4d9b-ad61-ec07fd215213
            type: User
            userConsentDescription: Access the application
            userConsentDisplayName: Access
            value: user_impersonation
    displayName: example-authorizing-app
```

example

```yaml
apiVersion: applications.azuread.upbound.io/v1beta1
kind: Application
metadata:
  annotations:
    meta.upbound.io/example-id: serviceprincipals/v1beta1/claimsmappingpolicyassignment
  labels:
    testing.upbound.io/example-name: example
  name: example
spec:
  forProvider:
    displayName: example
```

example

```yaml
apiVersion: applications.azuread.upbound.io/v1beta1
kind: Application
metadata:
  annotations:
    meta.upbound.io/example-id: applications/v1beta1/application
  labels:
    testing.upbound.io/example-name: example
  name: example
spec:
  forProvider:
    displayName: example-${Rand.RFC1123Subdomain}
```

example

```yaml
apiVersion: applications.azuread.upbound.io/v1beta1
kind: Application
metadata:
  annotations:
    meta.upbound.io/example-id: serviceprincipals/v1beta1/password
  labels:
    testing.upbound.io/example-name: example
  name: example
spec:
  forProvider:
    displayName: example
```

example

```yaml
apiVersion: applications.azuread.upbound.io/v1beta1
kind: Application
metadata:
  annotations:
    meta.upbound.io/example-id: serviceprincipals/v1beta1/certificate
  labels:
    testing.upbound.io/example-name: example
  name: example
spec:
  forProvider:
    displayName: example
```

example

```yaml
apiVersion: applications.azuread.upbound.io/v1beta1
kind: Application
metadata:
  annotations:
    meta.upbound.io/example-id: serviceprincipals/v1beta1/tokensigningcertificate
  labels:
    testing.upbound.io/example-name: example
  name: example
spec:
  forProvider:
    displayName: example
```

example

```yaml
apiVersion: applications.azuread.upbound.io/v1beta1
kind: Application
metadata:
  annotations:
    meta.upbound.io/example-id: applications/v1beta1/federatedidentitycredential
  labels:
    testing.upbound.io/example-name: example
  name: example
spec:
  forProvider:
    displayName: example
```

example

```yaml
apiVersion: applications.azuread.upbound.io/v1beta1
kind: Application
metadata:
  annotations:
    meta.upbound.io/example-id: serviceprincipaldelegated/v1beta1/permissiongrant
  labels:
    testing.upbound.io/example-name: example
  name: example
spec:
  forProvider:
    displayName: example
    requiredResourceAccess:
      - resourceAccess:
          - id: 37f7f235-527c-4136-accd-4a02d197296e
            type: Scope
          - id: e1fe6dd8-ba31-4d61-89e7-88639da4683d
            type: Scope
        resourceAppId: 00000003-0000-0000-c000-000000000000
```

example

```yaml
apiVersion: applications.azuread.upbound.io/v1beta1
kind: Application
metadata:
  annotations:
    meta.upbound.io/example-id: synchronization/v1beta1/job
  labels:
    testing.upbound.io/example-name: example
  name: example
spec:
  forProvider:
    displayName: example
    featureTags:
      - enterprise: true
        gallery: true
    templateId: 9c9818d2-2900-49e8-8ba4-22688be7c675
```

example

```yaml
apiVersion: applications.azuread.upbound.io/v1beta1
kind: Application
metadata:
  annotations:
    meta.upbound.io/example-id: serviceprincipals/v1beta1/principal
  labels:
    testing.upbound.io/example-name: example
  name: example
spec:
  forProvider:
    displayName: example
```

example

```yaml
apiVersion: applications.azuread.upbound.io/v1beta1
kind: Application
metadata:
  annotations:
    meta.upbound.io/example-id: applications/v1beta1/password
  labels:
    testing.upbound.io/example-name: example
  name: example
spec:
  forProvider:
    displayName: example
```

example-authorized-app

```yaml
apiVersion: applications.azuread.upbound.io/v1beta1
kind: Application
metadata:
  annotations:
    meta.upbound.io/example-id: applications/v1beta1/preauthorized
  labels:
    testing.upbound.io/example-name: example-authorized-app
  name: example-authorized-app
spec:
  forProvider:
    displayName: example-authorized-app
```

example

```yaml
apiVersion: applications.azuread.upbound.io/v1beta1
kind: Application
metadata:
  annotations:
    meta.upbound.io/example-id: applications/v1beta1/certificate
  labels:
    testing.upbound.io/example-name: example
  name: example
spec:
  forProvider:
    displayName: example
```

example

```yaml
apiVersion: applications.azuread.upbound.io/v1beta1
kind: Application
metadata:
  annotations:
    meta.upbound.io/example-id: synchronization/v1beta1/secret
  labels:
    testing.upbound.io/example-name: example
  name: example
spec:
  forProvider:
    displayName: example
    featureTags:
      - enterprise: true
        gallery: true
    templateId: 9c9818d2-2900-49e8-8ba4-22688be7c675
```