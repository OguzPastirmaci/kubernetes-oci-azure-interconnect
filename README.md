# Deploying a cross-cloud, GPU enabled Kubernetes cluster running on Oracle Cloud Infrastructure and Microsoft Azure using the interconnect

## Solution Overview

Oracle and Microsoft have built a dedicated, high-throughput, low-latency, private network connection between Azure and Oracle Cloud Infrastructure data centers. Customers can use the connection to securely transfer data at a high enough rate for offline handoffs and to support the performance required for primary applications that span the two clouds. Customers can access the connection by using either Oracle FastConnect or Microsoft ExpressRoute, and they don’t need to deal with configuration details or third-party carriers.

![](./images/oci-azure-interconnect.png)

The connection is currently available only in these areas:

- Between the Oracle Cloud Infrastructure location in the US East (Ashburn) region and the Azure Washington DC (East US) location.

- Between the Oracle Cloud Infrastructure location in the UK South (London) region and the Azure London (UK South) location.

**IMPORTANT:** This cross-cloud capability is currently in preview in Azure. To establish low latency connectivity between Azure and OCI, your Azure subscription must first be enabled for this capability.

You must enroll in the preview by completing this short [survey form](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyzVVsi364tClw522rL9tkpUMVFGVVFWRlhMNUlRQTVWSTEzT0dXMlRUTyQlQCN0PWcu).

You will receive an email back once your subscription has been enrolled. You aren't able to use the capability until you receive a confirmation email. You may also contact your Microsoft representative to be enabled for this preview. Access to the preview capability is subject to availability and restricted by Microsoft in its sole discretion. Completion of the survey does not guarantee access. 

This tutorial will walk you through deploying a cross-cloud Kubernetes cluster running on OCI and Azure using the interconnect. The tutorial also has instructions for deploying GPU virtual machines as Kubernetes workers, configuring them for Kubernetes, configuring the cluster for running MPI jobs, and setting up monitoring.

The tutorial environment consists of the following resources. You can deploy virtual machines with different configurations (e.g. VMs that have multiple GPUs) depending on your needs.

1. 1 [FastConnect](https://cloud.oracle.com/en_US/fastconnect) circuit on Oracle Cloud Infrastructure
2. 1 [ExpressRoute](https://azure.microsoft.com/en-us/services/expressroute/) circuit on Microsoft Azure
3. 1 [VM.Standard2.4](https://docs.cloud.oracle.com/iaas/Content/Compute/References/computeshapes.htm#virtualmachines) virtual machine running on OCI as the Kubernetes master
4. 1 [VM.GPU3.1](https://docs.cloud.oracle.com/iaas/Content/Compute/References/computeshapes.htm#virtualmachines) virtual machine running on OCI as a Kubernetes worker for GPU workloads
5. 1 [Standard NC6](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sizes-gpu#nc-series) virtual machine running on Microsoft Azure as a Kubernetes worker for GPU workloads.

## Step by step instructions for deploying a GPU enabled cross-cloud Kubernetes cluster running on Oracle Cloud Infrastructure and Microsoft Azure

### 1. [Configuring network](./docs/network-setup.md)
### 2. [Deploying the virtual machines](./docs/vm-deployment.md)
### 3. [Configuring the virtual machines](./docs/vm-setup.md)
### 3. [Deploying a Kubernetes cluster](./docs/kubernetes-setup.md)
### 4. [Deploying Kubeflow](./docs/kubeflow-setup.md)
### 5. [Deploying the Kubeflow MPI Operator](./docs/mpi-setup.md)
### 6. [Monitoring GPUs with Prometheus and Grafana](./docs/monitoring-setup.md)