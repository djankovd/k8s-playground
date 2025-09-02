# k8s-playground

Create kind cluster
```
kind create cluster --name k8s-playground --config kind-config.yaml
```

Bootstrap flux
```
flux bootstrap github \
  --token-auth \
  --owner=<USERNAME> \
  --repository=k8s-playground \
  --branch=main \
  --path=clusters/k8s-playground \
  --personal
```
