# Kubernetes Installation

## Overview

This section covers the installation of Kubernetes components required to bootstrap the cluster using `kubeadm`.

Installed components:
- kubeadm
- kubelet
- kubectl


## Add Kubernetes Repository

```bash
cat <<EOF | tee /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://pkgs.k8s.io/core:/stable:/v1.29/rpm/
enabled=1
gpgcheck=1
gpgkey=https://pkgs.k8s.io/core:/stable:/v1.29/rpm/repodata/repomd.xml.key
exclude=kubelet kubeadm kubectl
EOF
```

## Install Kubernetes Packages

```bash
dnf install -y kubelet kubeadm kubectl --disableexcludes=kubernetes
```

## Enable kubelet Service

```bash
systemctl enable --now kubelet
```

> kubelet may initially show errors until the control plane is initialized. This behavior is expected.


## Verify Installed Versions

```bash
kubeadm version
kubectl version --client
kubelet --version
```


## Validate kubelet Service

```bash
systemctl status kubelet
```

## Key Components

 kubeadm --> Cluster bootstrap tool 
 kubelet --> Node agent 
 kubectl --> Kubernetes CLI 


## Validation Commands

 Check kubelet --> `systemctl status kubelet` 
 Check kubeadm Version --> `kubeadm version` 
 Check kubectl Version --> `kubectl version --client`
 
## Learning Outcomes

- Kubernetes package installation
- kubelet service configuration
- kubeadm bootstrap preparation
- Kubernetes CLI usage
- Node-level Kubernetes services