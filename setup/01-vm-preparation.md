# VM Preparation

## Overview

This section covers the initial virtual machine setup required before installing Kubernetes.

The environment consists of:
- 1 Control Plane Node
- 1 Worker Node
- VMware-based virtual machines
- Rocky Linux 9.6

## VM Specifications

 Component -->  Master Node & Worker Node 
 CPU -->  2 vCPU fro each vm or we can say each node 
 RAM --> 4 GB RAM is alotted to master and 2 GB is alloted for worker 
 Disk  --> 20 GB eaach vm 

## Network Configuration

  Node     IP Address 
  master   192.168.56.10 
  worker   192.168.56.20 


## Hostname Configuration

### Master Node

```bash
hostnamectl set-hostname master
```

### Worker Node

```bash
hostnamectl set-hostname worker
```

---

## Hosts File Configuration

Configured `/etc/hosts` on both nodes:

```bash
127.0.0.1   localhost localhost.localdomain
::1         localhost localhost.localdomain

192.168.56.10 master
192.168.56.20 worker
```

## Disable Swap

```bash
swapoff -a
sed -i '/swap/d' /etc/fstab
```

## Configure SELinux

```bash
setenforce 0
sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config
```


## Enable Kernel Modules

```bash
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF
```

```bash
modprobe overlay
modprobe br_netfilter
```

## Configure Sysctl Parameters

```bash
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables  = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward                 = 1
EOF
```

Apply changes:

```bash
sysctl --system
```