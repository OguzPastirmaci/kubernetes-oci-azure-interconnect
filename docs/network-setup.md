# Step 1: Configuring network

----------
**IMPORTANT:**
This part of the guide is a work in progress. In the meantime, you can follow the instructions in the great blog post here:
https://medium.com/@j.jamalarif/how-to-setup-the-interconnect-between-oracle-cloud-infrastructure-and-microsoft-azure-da359233e5e9

-------


As a reminder, here is a table that lists the comparable networking components involved in each side of the connection.

![](../images/network-components-table.png)

# Prerequisite configuration

## Configuration on Azure

#### 1. Create an Azure Virtual Network (VNet)
Create a resource > Networking > Virtual network

#### 2. Create a Virtual Network Gateway

#### 3. Create Virtual Network Gateway

## Configuration on Oracle Cloud Infrastructure

#### 1. Create a Virtual Cloud Network (VCN)
Menu > Networking > Virtual Cloud Networking > Create a Virtual Cloud Network

#### 2. Create a Dynamic Routing Gateway (DRG)

Menu > Networking > Dynamic Routing Gateway > Create Dynamic Routing Gateway

# Creating the interconnection between OCI and Azure

After the prerequisites are completed, now you can create the interconnection between OCI and Azure.

#### 1. Setup Azure ExpressRoute

#### 2. Setup Oracle Cloud Infrastructure FastConnect

#### 3. Link Azure VNet to Azure ExpressRoute

#### 4. Associate Network Security groups and Route table to Azure VNet

#### 5. Configure OCI VCN Security Lists and Route Table

# Next Step

After you complete setting up the network, continue to [deploying the virtual machines](../docs/vm-deployment.md).