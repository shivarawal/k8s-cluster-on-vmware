# Control Plane Setup

## Overview

This section covers the initialization of the Kubernetes Control Plane using `kubeadm`.

The control plane is responsible for:
- API management
- Scheduling workloads
- Cluster state management
- Node orchestration


## Initialize Kubernetes Control Plane

```bash
kubeadm init \
--apiserver-advertise-address=192.168.56.10 \
--pod-network-cidr=192.168.0.0/16 \
--control-plane-endpoint=master
```


## Configure kubectl Access

After successful initialization:

```bash
mkdir -p $HOME/.kube
cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
chown $(id -u):$(id -g) $HOME/.kube/config
```

This allows the current user to interact with the cluster using `kubectl`.


## Verify Cluster Status

```bash
kubectl get nodes
```

Expected initial output:

```bash
master   NotReady
```

This is expected until the networking plugin is installed.


## Validate Control Plane Components

```bash
kubectl get pods -A
```

Core components include:
- kube-apiserver
- kube-controller-manager
- kube-scheduler
- etcd


## Verify API Server Health

```bash
curl -k https://master:6443/healthz
```

Expected output:

```bash
ok
```


## Verify API Server Port

```bash
ss -tulpn | grep 6443
```


## Key Validation Commands

 Check Cluster Nodes --> `kubectl get nodes` 
 Check System Pods --> `kubectl get pods -A` 
 Verify API Server --> `curl -k https://master:6443/healthz` 
 Verify API Port --> `ss -tulpn | grep 6443` 


## Learning Outcomes

- Kubernetes control plane bootstrap
- kubeadm initialization workflow
- API server validation
- kubectl configuration
- Kubernetes core component verification