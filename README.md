# k8s-playground

Create kind cluster
```
kind create cluster --name k8s-playground --config kind-config.yaml
```

Install flux
```
flux install --namespace=flux-system
```
