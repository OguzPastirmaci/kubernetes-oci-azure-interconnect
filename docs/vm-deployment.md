# Deploying the virtual machines

After your finished configuring the interconnect by using the instructions [in the guide here](../docs/network-setup.md), let's deploy the VMs that we will use as the Kubernetes master and workers.

To keep this guide simple, we will deploy a Kubernetes cluster that has a single master and 2 worker nodes. To keep this guide cost effective, we will deploy the smallest GPU VMs available in OCI and Azure. You may want to change the size of the VMs depending on your needs.

- 1 [VM.Standard2.4](https://docs.cloud.oracle.com/iaas/Content/Compute/References/computeshapes.htm#virtualmachines) virtual machine running on OCI as the Kubernetes master
- 1 [VM.GPU3.1](https://docs.cloud.oracle.com/iaas/Content/Compute/References/computeshapes.htm#virtualmachines) virtual machine running on OCI as a Kubernetes worker for GPU workloads
- 1 [Standard NC6](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sizes-gpu#nc-series) virtual machine running on Microsoft Azure as a Kubernetes worker for GPU workloads.

If you are familiar with deploying VMs in OCI and Azure, deploy 2 VMs in OCI and 1 VM in Azure with the info from the table below:

| Cloud Provider | VM name            | VM shape (size) | Region            |
| -------------- | ------------------ | --------------- | ----------------- |
| OCI            | oci-k8s-master     | VM.Standard2.4  | US East (Ashburn) |
| OCI            | oci-k8s-worker-1   | VM.GPU3.1       | US East (Ashburn) |
| Azure          | azure-k8s-worker-1 | Standard NC6    | East US           |


If you are not familiar with deploying VMs in OCI and Azure, follow the steps below:

## Deploying the VMs in OCI

Follow the steps below for deploying two VMs. 

1. Launch the OCI console and open the navigation menu. Under **Core Infrastructure**, go to **Compute** and click **Instances**.
2. Click **Create Instance.**
3. On the Create Instance page, enter the following:

- **Name your instance:** Enter `oci-k8s-master` for the first VM and then `oci-k8s-worker-1` for the second VM.

- **Choose an operating system or image source:** Ubuntu 18.04

- **Availability Domain:** Accept the default.

- **Instance Type:** Accept the default, Virtual Machine.

- **Instance Shape:** Choose `VM.Standard2.4` for `oci-k8s-master` and choose `VM.GPU3.1` for `oci-k8s-worker-1`. Choose `VM.Standard2.4` for `oci-k8s-master` and choose `VM.GPU3.1` for `oci-k8s-worker-1`.

- **Virtual cloud network compartment:** Select the compartment containing the cloud network that you created.
- **Virtual cloud network:** Select the cloud network that you created.
- **Subnet compartment:** Select the compartment containing the subnet that was created with your cloud network.
- **Subnet:** Select the subnet that was created with your cloud network.
Leave the Use network security groups to control traffic option cleared.
- To create a public IP address for the instance, select the **Assign a public IP address** option.
- **Boot volume:** Leave all the options cleared.
- **Add SSH Key:** Select Choose SSH key file. Click Choose Files, navigate to the location where you saved the public key portion (.pub) of the SSH key file that you created, select the file and click Open.


## Deploying the VM in Azure

1. Type **virtual machines** in the search.
2. Under **Services**, select **Virtual machines**.
3. In the **Virtual machines** page, select **Add**. The **Create a virtual machine** page opens.
4. In the **Basics** tab, under **Project details**, make sure the correct subscription is selected and then choose the same resource group with the ExpressRoute circuit you created.
5. Under **Instance details**, type `azure-k8s-worker-1` for the **Virtual machine name**, choose *East US* for your **Region**, and choose *Ubuntu 18.04 LTS* for your **Image**. Leave the other defaults.
6. Under **Administrator account**, select **SSH public key**, type `ubuntu` for your user name, then paste in your public key. Remove any leading or trailing white space in your public key.
7. In the **Networking** tab, select the **Virtual Network** and **Subnet** you created earlier in the [network setup guide](../docs/network-setup.md).
8. Leave the remaining defaults and then select the **Review + create** button at the bottom of the page.
9. On the **Create a virtual machine** page, you can see the details about the VM you are about to create. When you are ready, select **Create**.

# Next Step

After you finish deploying the VMs, continue to [configuring the virtual machines](./docs/vm-setup.md).