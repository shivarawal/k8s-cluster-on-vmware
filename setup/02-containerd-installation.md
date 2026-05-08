# containerd Installation

## Overview

This section covers the installation and configuration of `containerd` as the Kubernetes container runtime.

containerd was selected because:
- It is lightweight
- CNCF graduated
- Recommended for modern Kubernetes environments
- Widely used in production Kubernetes clusters

## Install Required Packages

```bash
dnf install -y yum-utils device-mapper-persistent-data lvm2
```

## Add Docker Repository

```bash
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```


## Install containerd

```bash
dnf install -y containerd.io
```


## Generate Default Configuration

```bash
mkdir -p /etc/containerd
containerd config default | tee /etc/containerd/config.toml
```

## Configure Systemd Cgroup Driver

Edit the configuration file:

```bash
vi /etc/containerd/config.toml
```

Update the following parameter:

```toml
SystemdCgroup = true
```

This configuration ensures compatibility between:
- kubelet
- systemd
- container runtime

## Restart containerd

```bash
systemctl restart containerd
systemctl enable containerd
```

## Verify containerd Status

```bash
systemctl status containerd
```

Expected output:

```bash
active (running)
```

## Configure crictl Runtime Endpoint

```bash
cat <<EOF | tee /etc/crictl.yaml
runtime-endpoint: unix:///run/containerd/containerd.sock
image-endpoint: unix:///run/containerd/containerd.sock
timeout: 10
debug: false
EOF
```

## Validate CRI Communication

```bash
crictl info
```


## Key Validation Checks

 Check Runtime Status --> `systemctl status containerd` 
 Check CRI Communication --> `crictl info` 
 Check Runtime Version --> `ctr version` 

---

## Learning Outcomes

- container runtime installation
- CRI configuration
- systemd cgroup configuration
- container runtime validation
- Kubernetes runtime preparation