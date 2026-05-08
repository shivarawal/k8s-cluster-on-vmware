# Cluster Validation

## Overview

This section covers the operational validation of the Kubernetes cluster after the control plane, worker node, and networking components were configured.

The goal of these checks is to verify:
- Cluster health
- Node communication
- API server availability
- Pod scheduling
- Networking functionality
- Runtime communication


# Verify Cluster Nodes

```bash
kubectl get nodes
```

Expected output:

```bash
master   Ready
worker   Ready
```


# Verify Detailed Node Information

```bash
kubectl get nodes -o wide
```

This command validates:
- Node registration
- Internal IP addresses
- Kubernetes versions
- Node roles


# Verify System Pods

```bash
kubectl get pods -A
```

Core components validated:

- kube-apiserver
- kube-controller-manager
- kube-scheduler
- etcd
- coredns
- calico-node
- kube-proxy

Expected pod state:

```bash
Running
```

# Verify API Server Health

```bash
curl -k https://master:6443/healthz
```

Expected output:

```bash
ok
```

# Verify kubelet Status

```bash
systemctl status kubelet
```

Expected state:

```bash
active (running)
```

# Verify container Runtime

```bash
systemctl status containerd
```


# Validate CRI Communication

```bash
crictl info
```



# Verify Kubernetes API Port

```bash
ss -tulpn | grep 6443
```

Expected result:

```bash
LISTEN
```

---

# Verify kubelet Port

```bash
ss -tulpn | grep 10250
```


# Verify Cluster DNS

Deploy validation pod:

```bash
kubectl run dns-test --image=busybox -- sleep 3600
```

Access pod:

```bash
kubectl exec -it dns-test -- sh
```

Test DNS:

```bash
nslookup kubernetes.default
```

# Verify Pod Scheduling

```bash
kubectl get pods -o wide
```

This validates:
- Pod scheduling
- Worker node workload placement
- Cluster orchestration


# Verify Node Communication

```bash
ping master
```

# Verify Cluster Events

```bash
kubectl get events -A
```

This helps identify:
- scheduling issues
- image pull failures
- networking problems
- node communication issues

# Cluster Validation Summary

 Control Plane -->Verified 
 Worker Node --> Verified 
 API Server --> Verified 
 kubelet --> Verified 
 containerd Runtime  --> Verified 
 Calico Networking --> Verified 
 DNS Resolution -->Verified 


# Learning Outcomes

- Kubernetes operational validation
- API server verification
- kubelet validation
- CRI runtime validation
- Kubernetes DNS testing
- Pod scheduling validation
- Cluster troubleshooting workflow