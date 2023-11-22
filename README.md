requirements - kind and helm

```    docker pull kindest/node:v1.28.0
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

