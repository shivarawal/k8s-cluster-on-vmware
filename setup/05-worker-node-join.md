## Worker Node Runtime Preparation

The worker node runtime environment was validated and configured before joining the cluster.

### Disable Swap

```bash
swapoff -a
sed -i '/swap/d' /etc/fstab
```


### Configure SELinux

```bash
setenforce 0
```

Verify status:

```bash
getenforce
```

Expected output:

```bash
Permissive
```


### Enable Kernel Modules

```bash
cat <<EOF | tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF
```

Load modules:

```bash
modprobe overlay
modprobe br_netfilter
```


### Configure Kubernetes Networking Parameters

```bash
cat <<EOF | tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables  = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward                 = 1
EOF
```

Apply changes:

```bash
sysctl --system
```


### Install Runtime Dependencies

```bash
dnf install -y yum-utils device-mapper-persistent-data lvm2
```


### Configure Docker Repository

```bash
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```


### Install containerd Runtime

```bash
dnf install -y containerd.io
```


### Generate Runtime Configuration

```bash
mkdir -p /etc/containerd
containerd config default | tee /etc/containerd/config.toml
```


### Configure Systemd Cgroup Driver

Update the following parameter in:

```bash
/etc/containerd/config.toml
```

```toml
SystemdCgroup = true
```


### Restart containerd

```bash
systemctl restart containerd
systemctl enable containerd
```

### Verify Runtime Status

```bash
systemctl status containerd
```

### Validate Runtime Communication

```bash
crictl info
```