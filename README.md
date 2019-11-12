# Kubernetes cluster spanning across Oracle Cloud Infrastructure and Microsoft Azure using the interconnect

## Solution Overview

### Audience and Business Problem Being Solved

Enterprises who operate data centers have long understood the value of standardizing on a smaller set of underlying components, including the operating system, to minimize operating costs and overhead. Customers and vendors alike have rallied around Kubernetes ahead of the alternatives, recognizing the value of an open (albeit somewhat complex) standard for container orchestration.

As enterprises have adopted container technology, they too have recognized the opportunity to build on this open Kubernetes platform, as a way to ease their transition from on-premises applications to the cloud, avoid lock-in across cloud providers, and provide the future fabric for hybrid deployments and even serverless applications.

As enterprises continue to evaluate the benefits of cloud, they are steadily adopting a multicloud strategy for various reasons, including disaster recovery, high availability, lower cost, and, most importantly, using the best services and solutions available in the market. To enable this diversification, customers interconnect cloud networks by using the internet, IPSec VPNs, or a cloud provider’s direct connectivity solution through the customer’s on-premises network. Interconnecting cloud networks can require significant investments in time, money, design, procurement, installation, testing, and operations, and it still doesn't guarantee a high-availability, redundant, low-latency connection.

Oracle and Microsoft recognize these customer challenges and have created a unified enterprise cloud for our mutual customers. Oracle and Microsoft have already done all the tedious, time-consuming work for you by providing low-latency, high-throughput connectivity between their two clouds. The rest of this post describes how to configure the network interconnection between Oracle Cloud Infrastructure and Microsoft Azure to create a secured, private, peered network between the two clouds.

Oracle and Microsoft have built a dedicated, high-throughput, low-latency, private network connection between Azure and Oracle Cloud Infrastructure data centers in the Ashburn, Virginia region that provides a data conduit between the two clouds. Customers can use the connection to securely transfer data at a high enough rate for offline handoffs and to support the performance required for primary applications that span the two clouds. Customers can access the connection by using either Oracle FastConnect or Microsoft ExpressRoute, and they don’t need to deal with configuration details or third-party carriers.

![](./images/oci-azure-interconnect.png)


- Secure private connection between the two clouds. No exposure to the internet.
- High availability and reliability. Built-in redundant 10-Gbps physical connections between the clouds.
- High performance, low latency, predictable performance compared to the internet or routing through an on-premises network.
- Straightforward, one-time setup.
- No intermediate service provider required to enable the connection.


The primary audiences of this solution guide are the system and network administrators responsible for the cloud infrastructure.

### Top-Level Value Proposition

This guide is a step-by-step walkthrough for configuring a Kubernetes cluster spanning across Oracle Cloud Infrastructure and Microsoft Azure using the network interconnection between Oracle Cloud Infrastructure and Microsoft Azure.

The Proof of Concept (PoC) environment consists of the following:

1. A dedicated network connection between Oracle Cloud Infrastructure (OCI) and Microsoft Azure (interconnect) using FastConnect on OCI side and ExpressRoute on Microsoft Azure side
2. Network configuration on OCI and Microsoft Azure
3. Virtual machines running on OCI and Microsoft Azure
4. Storage resources on OCI and Microsoft Azure
5. Security recommendations
6. Sample workload that will be deployed on the Kubernetes cluster

### TCO Analysis

This proof-of-concept environment is consisting of the following:

1. A [FastConnect](https://cloud.oracle.com/en_US/fastconnect) circuit on Oracle Cloud Infrastructure
2. An [ExpressRoute](https://azure.microsoft.com/en-us/services/expressroute/) circuit on Microsoft Azure
3. 1 [Standard D4s v3](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/sizes-general#dsv3-series-1) virtual machine running on Microsoft Azure as the Kubernetes master
4. 1 [VM.GPU3.1](https://docs.cloud.oracle.com/iaas/Content/Compute/References/computeshapes.htm#virtualmachines) virtual machine running on OCI as a Kubernetes worker for GPU workloads
5. 1 [VM.Standard2.4](https://docs.cloud.oracle.com/iaas/Content/Compute/References/computeshapes.htm#virtualmachines) virtual machine running on OCI as a Kubernetes worker for general purpose workloads


## Step by step instructions for deploying a GPU enabled cross-cloud Kubernetes cluster running on Oracle Cloud Infrastructure and Microsoft Azure

### 1. [Configuring network](/blob/master/docs/network-setup.md)
### 2. [Setting up the virtual machines](./blob/master/docs/kubeflow-setup.md)
### 3. [Deploying a Kubernetes cluster](./blob/master/docs/kubernetes-setup.md)
### 4. [Deploying Kubeflow](./blob/master/docs/kubeflow-setup.md)
### 5. [Deploying the Kubeflow MPI Operator](./blob/master/docs/mpi-setup.md)