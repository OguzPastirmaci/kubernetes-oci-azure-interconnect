# kubernetes-oci-azure-interconnect

As enterprises continue to evaluate the benefits of cloud, they are steadily adopting a multicloud strategy for various reasons, including disaster recovery, high availability, lower cost, and, most importantly, using the best services and solutions available in the market. To enable this diversification, customers interconnect cloud networks by using the internet, IPSec VPNs, or a cloud provider’s direct connectivity solution through the customer’s on-premises network. Interconnecting cloud networks can require significant investments in time, money, design, procurement, installation, testing, and operations, and it still doesn't guarantee a high-availability, redundant, low-latency connection.

Oracle and Microsoft recognize these customer challenges and have created a unified enterprise cloud for our mutual customers. Oracle and Microsoft have already done all the tedious, time-consuming work for you by providing low-latency, high-throughput connectivity between their two clouds. The rest of this post describes how to configure the network interconnection between Oracle Cloud Infrastructure and Microsoft Azure to create a secured, private, peered network between the two clouds.

Oracle and Microsoft have built a dedicated, high-throughput, low-latency, private network connection between Azure and Oracle Cloud Infrastructure data centers in the Ashburn, Virginia region that provides a data conduit between the two clouds. Customers can use the connection to securely transfer data at a high enough rate for offline handoffs and to support the performance required for primary applications that span the two clouds. Customers can access the connection by using either Oracle FastConnect or Microsoft ExpressRoute, as shown in Figure 1, and they don’t need to deal with configuration details or third-party carriers.

- Secure private connection between the two clouds. No exposure to the internet.
- High availability and reliability. Built-in redundant 10-Gbps physical connections between the clouds.
- High performance, low latency, predictable performance compared to the internet or routing through an on-premises network.
- Straightforward, one-time setup.
- No intermediate service provider required to enable the connection.
