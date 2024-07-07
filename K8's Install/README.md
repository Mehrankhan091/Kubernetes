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
To use the systemd cgroup driver in /etc/containerd/config.toml with runc delete all the configurations and set below configuratio9ns.

[plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc]
  [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc.options]
    SystemdCgroup = true

sudo systemctl restart containerd

Installing kubeadm, kubelet and kubectl 

These instructions are for Kubernetes v1.30.

Update the apt package index and install packages needed to use the Kubernetes apt repository:

sudo apt-get update
# apt-transport-https may be a dummy package; if so, you can skip that package
sudo apt-get install -y apt-transport-https ca-certificates curl gpg
Download the public signing key for the Kubernetes package repositories. The same signing key is used for all repositories so you can disregard the version in the URL:

# If the directory `/etc/apt/keyrings` does not exist, it should be created before the curl command, read the note below.
# sudo mkdir -p -m 755 /etc/apt/keyrings
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
Add the appropriate Kubernetes apt repository. Please note that this repository have packages only for Kubernetes 1.30; for other Kubernetes minor versions, you need to change the Kubernetes minor version in the URL to match your desired minor version (you should also check that you are reading the documentation for the version of Kubernetes that you plan to install).

# This overwrites any existing configuration in /etc/apt/sources.list.d/kubernetes.list
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
Update the apt package index, install kubelet, kubeadm and kubectl, and pin their version:

sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
(Optional) Enable the kubelet service before running kubeadm:

sudo systemctl enable --now kubelet
The kubelet is now restarting every few seconds, as it waits in a crashloop for kubeadm to tell it what to do.


Goto this link to create a cluster https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/
Just on the master Node only

sudo kubeadm init --pod-network-cidr=10.244.0.0/16  --apiserver-advertise-address=[ip add of master-node]
Note: Afret running this command you have to check some instructions of the output.


Now need to deploy pod network for the cluster

Click on the link for pod network addon mention on the output 
I will select the weave-net  and install it.

kubectl get ds -A 
kubectl edit ds weave-net -n kube-system
Now look for the container weave add another env variable
- name: IPALLOC_RANGE
  value: 10.244.0.0/16
