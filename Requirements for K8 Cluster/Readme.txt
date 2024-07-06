Purpose

Education
- Minikube
- Single Node Cluster with Kubeadm

Development and Testing
- Multi-node cluster with a Single Master and Multiple Workers
- Setup using Kubeadm tool or quick provision on Cloud (AWS, GCP, etc.)

Hosting Production Applications
- Highly Available Multi Node Cluster with Multiple Master Nodes
- Kubeadm, GCP, or Kops on AWS or other supported platforms
- Supports up to 5000 nodes, 150000 PODs, and 300000 containers with 100 pods per node

Resource Requirements
- Nodes
  
  | Nodes   | GCP                   | AWS                  |
  |---------|-----------------------|----------------------|
  | 1-5     | N1-Standard-1         | M3.medium            |
  | 6-10    | N1-Standard-2         | M3.large             |
  | 11-100  | N1-Standard-4         | M3.xlarge            |
  | 101-250 | N1-Standard-8         | M3.2xlarge           |
  | 251-500 | N1-Standard-16        | C4.4xlarge           |
  | >500    | N1-Standard-32        | C4.8xlarge           |

Cloud or On-Premise
- Use Kubeadm for On-Premise
- GKE for GCP
- Kops for AWS
- Azure Kubernetes Service (AKS) for Azure

Nodes
- Virtual or Physical Machines
- Minimum of 4 Node Cluster
- Master vs Worker nodes
- Linux x86_64 Architecture

Note:
- Typically, all control plane components are on the master node. However, in large clusters, you may choose to separate the ETCD cluster to its own nodes.

Workloads
- Storage
  - High Performance: SSD-backed Storage
  - Multiple Concurrent Connections: Network-based Storage
  - Persistent shared volumes for shared access across multiple PODs
  - Label nodes with specific disk types
  - Use Node Selectors to assign applications to nodes with specific disk types

- Applications
  - How many applications are hosted?
  - What kind of applications are hosted?
    - Web
    - Big Data/Analytics

- Application Resource Requirements
  - CPU
  - Memory

- Traffic
  - Continuous Heavy Traffic
  - Burst Traffic

