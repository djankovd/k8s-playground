# k8s-playground

Create kind cluster
```
kind create cluster --name k8s-playground --config kind-config.yaml
```

if using rootless docker or podman:
```
systemd-run --scope --user -p "Delegate=yes" kind create cluster
```
more info: [rootless configuration](https://kind.sigs.k8s.io/docs/user/rootless/)

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

Check subnetwork for metallb
```
docker network inspect -f '{{.IPAM.Config}}' kind
```

Create helmrepository
```
flux create source helm podinfo \
--namespace=flux-system \
--url=https://stefanprodan.github.io/podinfo \
--interval=24h --export > podinfo-helmrepo.yaml
```

Create file locally
```
cat > podinfo-values.yaml <<EOL
replicaCount: 2
resources:
  limits:
    memory: 256Mi
  requests:
    cpu: 100m
    memory: 64Mi
EOL
```

Create helmrelease
```
flux create helmrelease podinfo \
--namespace=podinfo \
--source=HelmRepository/podinfo \
--release-name=podinfo \
--chart=podinfo \
--chart-version=">5.0.0" \
--values=podinfo-values.yaml --export > podinfo-helmlrelease.yaml
```
