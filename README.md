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

This guide will provide step-by-step instructions to deploy a Kubernetes cluster that spans across Oracle Cloud Infrastructure and Microsoft Azure using the OCI-Azure interconnect.

### Top-Level Value Proposition

This guide is a step-by-step walkthrough for configuring a Kubernetes cluster spanning across Oracle Cloud Infrastructure and Microsoft Azure using the network interconnection between Oracle Cloud Infrastructure and Microsoft Azure.

