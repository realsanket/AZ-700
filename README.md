# AZ-700(Azure Network Engineer Associate)

The goal of this repository is to serve as a comprehensive study guide for the AZ-700 exam. This is a work in progress and will be updated continuously as I cover more exam objectives. It includes detailed notes and explanations for each topic based on my understanding. Contributions are welcome; feel free to create a pull request if you wish to add content or correct any errors.

## **Virtual Networks (VNet)**  

A virtual network is a logical representation of your own network. Virtual networks can be used in cloud and hybrid cloud scenarios. When creating a virtual network, you will need to define the address space and create at least one subnet. Subnets in the virtual network subnet must have a unique address range that does not overlap with other subnets. Also consider traffic control, resource isolation, and topology.Azure VNet enables resources in Azure to securely communicate with each other, the internet, and on-premises networks.

- **Capabilities of Azure Virtual Networks**
  - **Communication with the internet**: All resources in a VNet can communicate outbound to the internet by default. You can communicate inbound to a resource by assigning a public IP address or a public Load Balancer. You can also use public IP or public Load Balancer to manage your outbound connections.
  - **Communication between Azure resources**: There are three key mechanisms through which Azure resources can communicate: VNets, VNet service endpoints, and VNet peering. Virtual Networks can connect not only VMs but other Azure resources, such as the App Service Environment, Azure Kubernetes Service, and Azure VM Scale sets. You can use service endpoints to connect to other Azure resource types, such as Azure SQL databases and storage accounts. When you create a VNet, your services and VMs within your VNet can communicate directly and securely with each other in the cloud.
  - **Communication between on-premises resources**: Securely extend your data center. You can connect your on-premises computers and networks to a virtual network using any of the following options: Point-to-site virtual private network (VPN), Site-to-site VPN, Azure ExpressRoute.
  - **Filtering network traffic**: You can filter network traffic between subnets using any combination of network security groups and network virtual appliances like firewalls, gateways, proxies, and Network Address Translation (NAT) services.
  - **Routing network traffic**: Azure routes traffic between subnets, connected virtual networks, on-premises networks, and the Internet by default. You can implement route tables or border gateway protocol (BGP) routes to override the default routes Azure creates.
  
**Enterprise Relevance**: Core component for any cloud architecture.

![alt text](./images/image.png)

### **Virtual Network Address Space**

Virtual network address space is the range of IP addresses that you can use for your resources in a virtual network. When you create a virtual network, you must specify a custom private IP address space using public and private (RFC 1918) addresses. Azure assigns resources in a virtual network a private IP address from the address space that you assign. For example, if you deploy a VM in a VNet with address space

### **Private IP Address Space (RFC 1918)**

When creating a VNet, you must specify a custom private IP address space using public and private (RFC 1918) addresses. Azure assigns resources in a virtual network a private IP address from the address space that you assign. For example, if you deploy a VM in a VNet with address space 10.0.0.0/16, the VM will be assigned a private IP like 10.0.0.4.

#### **RFC 1918 Address Ranges:**

- **10.0.0.0 - 10.255.255.255** (10/8 prefix)
- **172.16.0.0 - 172.31.255.255** (172.16/12 prefix)
- **192.168.0.0 - 192.168.255.255** (192.168/16 prefix)

#### **Azure Reserved IP Addresses:**

- **x.x.x.0**: Network address
- **x.x.x.1**: Reserved by Azure for the default gateway
- **x.x.x.2, x.x.x.3**: Reserved by Azure to map the Azure DNS IPs to the VNet space
- **x.x.x.255**: Network broadcast address

#### **Unavailable Address Ranges:**

- **224.0.0.0/4** (Multicast)
- **255.255.255.255/32** (Broadcast)
- **127.0.0.0/8** (Loopback)
- **169.254.0.0/16** (Link-local)
- **168.63.129.16/32** (Internal DNS)

## Subnets

A virtual network can be segmented into one or more subnets Subnets provide logical divisions within your network Subnets can help improve security, increase performance, and make it easier to manage the network
Each subnet must have a unique address range – cannot overlap with other subnets in the virtual network in the subscription
✔️  Azure reserves 5 IP addresses within each subnet.

You can segment VNets into smaller subnets, creating as many subnets as you require within the subscription limit. You can then deploy Azure resources in a specific subnet. Just like in a traditional network, subnets allow you to segment your VNet address space into segments that are appropriate for the organization's internal network. This also improves address allocation efficiency.

When planning to implement subnets, consider the following:

- Each subnet must have a unique address range, specified in Classless Inter-Domain Routing (CIDR) format.
- Certain Azure services require their own subnet.
- Subnets can be used for traffic management. For example, you can create subnets to route traffic through a network virtual appliance.
- You can limit access to Azure resources to specific subnets with a virtual network service endpoint. You can create multiple subnets and enable a service endpoint for some subnets, but not others.
- You can secure resources within subnets using Network Security Groups.

**Enterprise Relevance**: Organizing resources, managing traffic, and security boundaries.

### Azure Reserved IP Addresses:**

- **x.x.x.0**: Network address
- **x.x.x.1**: Reserved by Azure for the default gateway
- **x.x.x.2, x.x.x.3**: Reserved by Azure to map the Azure DNS IPs to the VNet space
- **x.x.x.255**: Network broadcast address

**Enterprise Relevance**: Organizing resources, managing traffic, and security boundaries.

![alt text](./images/image%20copy.png)

## Understand Regions and Subscriptions

### Regions

A VNet is scoped to a single region/location; however, multiple virtual networks from different regions can be connected using Virtual Network Peering.
[Learn more about Microsoft's global network](https://docs.microsoft.com/azure/networking/microsoft-global-network)  
[Learn more about VNet planning and design](https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm#connectivity)

### Subscriptions

All Azure resources are created in an Azure region and subscription. A resource can only be created in a virtual network that exists in the same region and subscription as the resource. However, you can connect virtual networks that exist in different subscriptions and regions. When deciding which region(s) to deploy resources in, consider where consumers of the resources are physically located:

- **Network Latency**: Consumers of resources typically want the lowest network latency to their resources. To determine relative latencies between a specified location and Azure regions, see [View relative latencies](https://docs.microsoft.com/azure/network-watcher/view-relative-latencies?toc=/azure/virtual-network/toc.json).
- **Data Residency, Sovereignty, Compliance, or Resiliency Requirements**: If you have such requirements, choosing the region that aligns with them is critical. For more information, see [Azure geographies](https://azure.microsoft.com/global-infrastructure/geographies/).
- **Resiliency Across Availability Zones**: Do you require resiliency across Azure Availability Zones within the same Azure region for the resources you deploy? You can deploy resources, such as virtual machines (VM), to different availability zones within the same virtual network. Not all Azure regions support availability zones. To learn more about availability zones and the regions that support them, see [Availability zones](https://docs.microsoft.com/azure/availability-zones/az-overview?toc=/azure/virtual-network/toc.json).

### Regional/Global Presence

Azure datacenters exist in many regions worldwide. When selecting multiple Azure datacenters, consider two related factors: geographical distances and latency. To optimize user experience, evaluate the distance between each virtual datacenter as well as the distance from each virtual datacenter to the end users. An Azure region that hosts your virtual datacenter must conform with regulatory requirements of any legal jurisdiction under which your organization operates.

![alt text](./images/image%20copy%202.png)

## Create a Virtual Network

Create a virtual network using the Azure portal - [Azure Portal QuickStart](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

✔️ Always plan to use an address space that is not already in use in your organization, either on-premises or in other VNets. Even if you plan for a VNet to be cloud-only, you may want to make a VPN connection to it later. If there is any overlap in address spaces at that point, you will have to reconfigure or recreate the VNet. The next lesson will focus on IP addressing.

### Best Practices

As you build your network in Azure, it is important to keep in mind the following universal design principles:

- **Ensure non-overlapping address spaces**: Make sure your VNet address space (CIDR block) does not overlap with your organization's other network ranges.
- **Reserve address space for the future**: Your subnets should not cover the entire address space of the VNet. Plan ahead and reserve some address space for the future.
- **Fewer large VNets**: It is recommended you have fewer large VNets rather than multiple small VNets. This will prevent management overhead. (5 IP addresses are not usable per subnet due to Azure reserving them)
- **Secure your VNets**: Secure your VNets by assigning Network Security Groups (NSGs) to the subnets beneath them.

### Create a Virtual Network using Azure CLI

```bash
# filepath: /path/to/azure-cli/commands.sh
az group create --name example-resources --location eastus

az network vnet create \
  --name example-vnet \
  --resource-group example-resources \
  --address-prefix 10.0.0.0/16

az network vnet subnet create \
  --address-prefix 10.0.1.0/24 \
  --name example-subnet \
  --resource-group example-resources \
  --vnet-name example-vnet
```

### Create a Virtual Network using Bicep

```bicep
// filepath: /path/to/bicep/main.bicep
resource rg 'Microsoft.Resources/resourceGroups@2021-04-01' = {
  name: 'example-resources'
  location: 'East US'
}

resource vnet 'Microsoft.Network/virtualNetworks@2021-02-01' = {
  name: 'example-vnet'
  location: rg.location
  properties: {
    addressSpace: {
      addressPrefixes: [
        '10.0.0.0/16'
      ]
    }
  }
}

resource subnet 'Microsoft.Network/virtualNetworks/subnets@2021-02-01' = {
  name: 'example-subnet'
  parent: vnet
  properties: {
    addressPrefix: '10.0.1.0/24'
  }
}
```

### Create a Virtual Network using Terraform

```txt
# filepath: /path/to/terraform/main.tf
provider "azurerm" {
  features {}
}

resource "azurerm_resource_group" "example" {
  name     = "example-resources"
  location = "East US"
}

resource "azurerm_virtual_network" "example" {
  name                = "example-vnet"
  address_space       = ["10.0.0.0/16"]
  location            = azurerm_resource_group.example.location
  resource_group_name = azurerm_resource_group.example.name
}

resource "azurerm_subnet" "example" {
  name                 = "example-subnet"
  resource_group_name  = azurerm_resource_group.example.name
  virtual_network_name = azurerm_virtual_network.example.name
  address_prefixes     = ["10.0.1.0/24"]
}
```

![alt text](./images/image%20copy%203.png)

