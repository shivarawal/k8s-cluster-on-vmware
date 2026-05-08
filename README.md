# Kubernetes Cluster on VMware using kubeadm

## Project Overview

This project demonstrates the setup of a production-style 2-node Kubernetes cluster using kubeadm on VMware-based Rocky Linux virtual machines.

The cluster includes:
- Control Plane Node
- Worker Node
- containerd runtime
- Calico networking
- nginx workload deployment


## Technologies Used


 Virtualization --> VMware 
 OS --> Rocky Linux 9.6 
 Container Runtime --> containerd 
 Orchestration --> Kubernetes v1.29 
 Networking --> Calico 
 Deployment Tool --> kubeadm 


## Setup Workflow

1. VM Preparation
2. Kernel & Network Configuration
3. containerd Installation
4. Kubernetes Package Installation
5. Control Plane Initialization
6. Calico Networking Setup
7. Worker Node Join
8. Cluster Validation
9. nginx Deployment


## Cluster Validation

```bash
kubectl get nodes
kubectl get pods -A
