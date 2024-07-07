Installing Kubeadm  https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/
Installing a container runtime https://kubernetes.io/docs/setup/production-environment/container-runtimes/#containerd
Enable IPv4 packet forwarding
# sysctl params required by setup, params persist across reboots
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.ipv4.ip_forward = 1
EOF

# Apply sysctl params without reboot
sudo sysctl --system

Verify that net.ipv4.ip_forward is set to 1 with:
sysctl net.ipv4.ip_forward

After doing this select containerD https://kubernetes.io/docs/setup/production-environment/container-runtimes/#containerd
Getting Started wirh containerD https://github.com/containerd/containerd/blob/main/docs/getting-started.md
 Here in the git repo searach for option 2 and it will refer to the page https://docs.docker.com/engine/install/ubuntu/

 # Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

# Inatalling ContainerD
sudo apt-get install containerd.io  
systemctl status containerd

# Cgroup driver section
1. cgroupfs (default)
2. systemd
to check which cgroup driver is running
ps -p 1

Configuring the systemd cgroup driver
To use the systemd cgroup driver in /etc/containerd/config.toml with runc, set
[plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc]
  [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc.options]
    SystemdCgroup = true

