# Metal Kubernetes Cluster Setup

## k3s

Initial Cluster Node:

```sh
curl -sfL https://get.k3s.io | sh -s - \
  --flannel-backend=none \
  --disable-network-policy \
  --disable-kube-proxy \
  --egress-selector-mode=disabled \
  --cluster-cidr=10.42.0.0/16,2001:678:b7c:600::/56 \
  --service-cidr=10.43.0.0/16,2001:678:b7c:43::/108 \
  --disable=traefik \
  --disable=servicelb \
  --tls-san "mk1.ffddorf.net" \
  --cluster-init
```

Add node to cluster:

```sh
read -s K3S_TOKEN
export K3S_TOKEN
export K3S_URL="https://[fd00:7::2e76:8aff:fe5d:18b0]:6443"
curl -sfL https://get.k3s.io | sh -s -
```

## Cilium

```sh
helm repo add cilium https://helm.cilium.io/
helm upgrade --install cilium cilium/cilium --namespace cilium --create-namespace --values cilium-values.yaml
```

## Rook

```sh
helm repo add rook-release https://charts.rook.io/release
```

Operator:

_uses default values_

```sh
helm upgrade --install \
  --create-namespace --namespace rook-ceph \
  rook-ceph \
  rook-release/rook-ceph
```

Cluster:

```sh
helm upgrade --install \
  --create-namespace --namespace rook-ceph \
  --values rook-cluster-values.yaml \
  rook-ceph-cluster \
  rook-release/rook-ceph-cluster
```

## MetalLB

## Nginx Ingress

## Cert Manager

## Prometheus Stack & Loki
