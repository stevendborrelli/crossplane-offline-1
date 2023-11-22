# requirements - kind and helm

``` 
    docker pull kindest/node:v1.28.0
    kind create cluster --name=crossplane --image=kindest/node:v1.28.0 --config=cluster.yaml
    docker pull xpkg.upbound.io/crossplane-contrib/provider-helm:v0.15.0
    docker pull crossplane/crossplane:v1.12.0
    kind load docker-image xpkg.upbound.io/crossplane-contrib/provider-helm:v0.15.0 --name=crossplane
    kubectl create ns crossplane-system
    kind load docker-image crossplane/crossplane:v1.12.0 --name=crossplane
    kubectl create -f pv.yaml
    kubectl create -f pvc.yaml
    helm install crossplane -n crossplane-system ./temp/crossplane
    kubectl get pods // to make sure crossplane was deployed successfully
    kubectl apply -f provider.yml
    kubectl get providerrevision
```

## The result will be:

```
Name:         provider-helm-xpkg-upbound
Namespace:    
Labels:       pkg.crossplane.io/package=provider-helm
Annotations:  <none>
API Version:  pkg.crossplane.io/v1
Kind:         ProviderRevision
Metadata:
  Creation Timestamp:  2023-11-22T14:16:13Z
  Finalizers:
    revision.pkg.crossplane.io
  Generation:  1
  Managed Fields:
    API Version:  pkg.crossplane.io/v1
    Fields Type:  FieldsV1
    fieldsV1:
      f:metadata:
        f:finalizers:
          .:
          v:"revision.pkg.crossplane.io":
        f:labels:
          .:
          f:pkg.crossplane.io/package:
        f:ownerReferences:
          .:
          k:{"uid":"47ac5d30-324b-4a88-a0e8-704d7afe8539"}:
      f:spec:
        .:
        f:desiredState:
        f:ignoreCrossplaneConstraints:
        f:image:
        f:packagePullPolicy:
        f:revision:
        f:skipDependencyResolution:
        f:webhookTLSSecretName:
    Manager:    crossplane
    Operation:  Update
    Time:       2023-11-22T14:16:13Z
  Owner References:
    API Version:           pkg.crossplane.io/v1
    Block Owner Deletion:  true
    Controller:            true
    Kind:                  Provider
    Name:                  provider-helm
    UID:                   47ac5d30-324b-4a88-a0e8-704d7afe8539
  Resource Version:        1137
  UID:                     57a0c28b-dd2e-4c0a-a270-b052d9097035
Spec:
  Desired State:                  Active
  Ignore Crossplane Constraints:  false
  Image:                          xpkg.upbound.io/crossplane-contrib/provider-helm:v0.15.0
  Package Pull Policy:            Never
  Revision:                       1
  Skip Dependency Resolution:     false
  Webhook TLS Secret Name:        webhook-tls-secret
Events:
  Type     Reason             Age                From                                         Message
  ----     ------             ----               ----                                         -------
  Normal   BindClusterRole    62s (x3 over 62s)  rbac/providerrevision.pkg.crossplane.io      Bound system ClusterRole to provider ServiceAccount(s)   
  Warning  ParsePackage       1s (x7 over 62s)   packages/providerrevision.pkg.crossplane.io  failed to get pre-cached package with pull policy Never  
  Warning  ApplyClusterRoles  0s (x7 over 61s)   rbac/providerrevision.pkg.crossplane.io      cannot apply ClusterRole: cannot create object: ClusterRole.rbac.authorization.k8s.io "crossplane:provider:provider-helm-xpkg-upbound:system" is invalid: rules[0].apiGroups: Required value: resource rules must supply at least one api group

```