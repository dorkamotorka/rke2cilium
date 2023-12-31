# rke2cilium
RKE2 Kubernetes Cluster with Cilium

Tested on RKE2 version `v1.26.9+rke2r1`.

## Install

General install:
```
curl -sfL https://get.rke2.io | sudo sh -
sudo systemctl enable rke2-server.service
sudo systemctl start rke2-server.service
```

To install a specific version:
```
sudo su
curl -sfL https://get.rke2.io --output install.sh
chmod +x install.sh
INSTALL_RKE2_CHANNEL="v1.24" ./install.sh
```

## Deployment

Deploy example pods:
```
kubectl apply -f deployment.yaml
```

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

## MetalLB

Apply metalLB CRD definition and create the namespace for it (PodSecurityPolicy will fail but for POC this is not needed):
```
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.11.0/manifests/namespace.yaml
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.11.0/manifests/metallb.yaml
```

Then apply metalLB config (address pool it can use):
```
kubectl apply -f metallb-config.yaml
```

and check the metalLB controller logs using `kubectl logs -n metallb-system controller-*` and you should see something like:
```
{"caller":"level.go:63","event":"serviceUpdated","level":"info","msg":"updated service object","service":"default/cilium-gateway-my-gateway","ts":"2023-09-13T08:37:09.801255257Z"}
{"caller":"level.go:63","event":"ipAllocated","ip":"88.200.23.240","level":"info","msg":"IP address assigned by controller","service":"kube-system/cilium-ingress","ts":"2023-09-13T08:37:09.803838006Z"}
{"caller":"level.go:63","event":"serviceUpdated","level":"info","msg":"updated service object","service":"kube-system/cilium-ingress","ts":"2023-09-13T08:37:09.80886427Z"}
```
