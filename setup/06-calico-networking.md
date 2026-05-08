# Calico Networking Setup

## Overview

This section covers the installation and validation of Calico as the Container Network Interface (CNI) plugin for the Kubernetes cluster.

Calico provides:
- Pod-to-pod networking
- Network policy enforcement
- Cluster communication
- Scalable Kubernetes networking

## Why Calico

Calico was selected because:
- Production-grade networking solution
- Widely used in Kubernetes environments
- Supports network policies
- Lightweight and scalable
- CNCF ecosystem adoption



## Install Calico CNI

Run the following command on the control plane node:

```bash
kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.28.0/manifests/calico.yaml
```


## Verify Calico Components

```bash
kubectl get pods -A
```

Expected Calico components:

- calico-node
- calico-kube-controllers


## Verify Node Status

```bash
kubectl get nodes
```

Expected output:

```bash
master   Ready
worker   Ready
```

## Verify Cluster Networking

### Check Cluster Pods

```bash
kubectl get pods -A
```

All system pods should eventually move to:

```bash
Running
```

## Verify kube-proxy

```bash
kubectl get pods -n kube-system
```

Expected components:
- kube-proxy
- calico-node
- coredns

---

## Validate Pod Networking

### Deploy Test Pod

```bash
kubectl run network-test --image=busybox -- sleep 3600
```


### Verify Pod Status

```bash
kubectl get pods
```

---

### Access Test Pod

```bash
kubectl exec -it network-test -- sh
```
### Test DNS Resolution

Inside the pod:

```bash
nslookup kubernetes.default
```


### Test Cluster Connectivity

Inside the pod:

```bash
ping kubernetes.default.svc.cluster.local
```


## Key Validation Commands

 Verify Nodes --`kubectl get nodes` 
 Verify Pods --> `kubectl get pods -A` 
 Verify Calico Pods --> `kubectl get pods -n kube-system`
 Verify DNS --> `nslookup kubernetes.default` 
 Verify Networking --> `ping kubernetes.default.svc.cluster.local` 


## Learning Outcomes

- Kubernetes CNI configuration
- Cluster networking setup
- Pod-to-pod communication
- Kubernetes DNS validation
- Calico networking validation
- Kubernetes network troubleshooting