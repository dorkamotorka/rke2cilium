# rke2cilium
RKE2 Kubernetes Cluster with Cilium


## Hubble
```
cilium status
cilium hubble port-forward&
hubble status
hubble observe
```

## Gateway API 

Install CRD (check latest version [here](https://github.com/kubernetes-sigs/gateway-api)):
```
kubectl apply -f https://github.com/kubernetes-sigs/gateway-api/releases/download/v0.8.0/experimental-install.yaml
```
