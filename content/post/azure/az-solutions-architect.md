---
title: "Az Solutions Architect"
date: 2019-08-30T18:18:26-04:00
category: azure
---

compute, network, storage, security  

### Deploy and configure infrastructure  

#### Analyze resource utilization and consumption  

* configure diagnostic settings on resources  
* create baseline for resources  
* create and rest alerts  
* analyze alerts across subscription  
* analyze metrics across subscription  
* create action groups  
* monitor for unused resources  
* monitor spen  
* report on spend  
* utilize Log Search query functions  
* view alerts in Azure Monitor logs  

#### Create and configure storage accounts  

* configure network access to the storage account  
* create and configure storage account  
* generate shared access signature  
* install and use Azure Storage Explorer  
* manage access keys  
* monitor activity log by using Azure Monitor logs  
* implement Azure storage replication  

#### Create and configure a VM for Windows and Linux  

* configure high availability  
* configure monitoring, networking, storage, and virtual machine size  
* deploy and configure scale sets  

#### Automate deployment of VMs  

* Modify Azure Resource Mnager template  
* configure location of new VMs  
* configure VHD template  
* deploy from template  
* save a deployment as an Azure Resource Manager template  
* deploy Windows and Linux VMs  

#### Implement solutions that use virtual machines (VMs)  

* provision VMs  
* create Azure Resource Manager templates  
* configure Azure Disk Encryption for VMs  

#### Create connectivity between virtual networks  

* create and configure VNET peering  
* create and configure VNET to VNET  
* veriy virtual network connectivity  
* create virtual network gateway  

#### Implement and manage virtual networking  

* configure private and public IP addresses, network routes, network interface, subnets, and virtual network  

#### Manage Azure Aztive Directory (AD)  
* add custom domains  
* configure Azure AD Identity Portection, Azure AD Join, and Enterprise State Roaming  
* configure self-service password reset  
* implement conditional access policies  
* manage multiple directories  
* perform an access review  

#### Implement and manage hybrid identities  
* install and configure Azure AD Connect  
* configure federation and single sign-on  
* manage Azure AD Connect  
* manage password sync and writeback  


***  

# Manage Resources in Azure  

## Align requirements with cloud types and service models in Azure  

Azure supports 3 approaches to deploying cloud resources:  
1. public  
2. private  
3. hybrid  

Selecting between them will change several factors of the services you move into Azure inlcuding cost, maintenance requirements, and security. It is important to make an informed descision on which model to leverage for your services. We will also look at the Azure supported service models, which will help in determining the services you should start with when planning out an Azure deployment.  

<img src="/images/2-cloud-deployment.png" alt="cloud deployment types" style="width: 305px;">  

* Public Cloud   
    - most common  
    - services offered over the public internet and available to whoever wants to purchase them  
    - cloud resources (servers, storage, etc) are owned and operated by a 3rd party cloud service provider and delivered over the internet.  
    - Services may be free or sold on demand  
        - allows customers to pay only per suage for the CPU cycles, storage, or bandwidth they consume  

* Why public cloud?  
    - can be deployed faaster than on-premises infrastructures and with an almost infitiely scalable platform  
    - every employee of a company can use the same application from any office or branch using their device of choice via the internet  

Examples of why you'd use the public cloud:  

* **Sevice consumption through on-demand or subscription model:** The on-demand or subscription model allows you to pay for the portion of CPU, storage, and other resources that you use or reserve.  

* **No up-front investment of hardware:** No requirement to purchase, manage, and maintain on-premises hardware and application infrastructure. The cloud service provider is held responsible for all management and maintenance of the system.  

* **Automation:** Quickly provision infrastructure resources using a web portal, scripts, or via automation.  

* **Geographic dispersity:** Store data near your users, or in desired locations without having to maintain your own datacenters.  

* **Reduced hardware maintenance:** The service provider is responsible for hardware maintenance.  

* Private Cloud
    - consists of computing resources used exclusively by users from one business or organization.  
    - can be physically located at your organization's on-site datacenter, or it can be host by a 3rd-party service provider.  
    - uses on-premises infrastructure and services to provide similar benefits of the public cloud  
    - uses an abstraction platform to provide *cloud-like* services such as Kubernetes clusters or a complete cloud environment like Azure Stack.  
    - the organization is responsible for the purchase, config, and maintenance of the hardware.  
    - communication between the systems is usually on the network infrastructure that the business owns and maintains. For example, a private internal network or a dedicated fiber optic connection between buildings.  

* Why private cloud?  
    - can provide more flexibility to an organization  
    - your organization can customize its cloud environment to meet specific business needs.  
    - since resources are not shared with others, high levels of control and security are possible.  
    - private clouds can provide a level of scalability and efficiency.  

Examples of why you would use private cloud:  
* **Pre-existing environment:** An existing operating environment that can't be replicated in the public cloud. A large investment in hardware and employees with solution expertise. A large organization may choose to commoditize their computing resources.  

* **Legacy applications:** Business-critical legacy applications that can't easily be physically relocated.  

* **Data sovereignty and security:** Political borders and legal requirements may dictate where data can physically exist.  

* **Regulatory compliance / certification:** PCI or HIPAA compliance. Certified on-premises datacenter.  

* Hybrid Cloud  
    - combines a public cloud and a private cloud by allowing data and applications to be shared between them.  
    - when computing and processing demand flucuates, hybrid cloud computing gives businesses the ability to seemlessly scale their on-premises infrastructure up to the public cloud to handle any overflow - without giving 3rd-party datacenters access to the entirety of their data  
    - Organizations gain the flexibility and computing power of the public cloud for basic and non-sensitive computing tasks, while keeping business-critical applications and data on-premises, safely behind a company firewall.  
    - using hybrid cloud helps eliminate the need to make up-front capital expenditures  to handle short-term spikes in demand.  
    - companies only pay for resources they temporarily use instead of having to purchase, program, and maintain additional resources and equipment that could remain idle over long periods of time.  
    - integration is usually through a secure VPN between cloud providers like Azzure and on-premies datacenters  

Why hybrid cloud?  
* Hybrid cloud allows your organization to control and maintain a private infrastructure for sensitive assets.  
* It also gives you the flexibility to take advantage of additional resources in the public cloud when you need them.  
* With the ability to scale to the public cloud, you pay for extra computing power only when needed.  
* can ease the transition to the cloud  
    - you can migrate gradually by phasing in workloads over time.  

Examples of why you'd use hybrid cloud:  

* **Existing hardware investment:** Business reasons require that you use an existing operating environment and hardware.  

* **Regulatory requirements:** Regulation requires that the data needs to remain at a physical location.  

* **Unique operating environment:** Public cloud can't replicate a legacy operating environment.  

* **Migration:** Move workloads to the cloud over time.  

# Service Models  

Cloud computing resources are delivered using three different service models.  

**Infrastructure-as-a-service (IaaS)** provides instant computing infrastructure that you can provision and manage over the Internet.  
**Platform as a service (PaaS)** provides ready-made development and deployment environments that you can use to deliver your own cloud services.  
**Software as a service (SaaS)** delivers applications over the Internet as a web-based service.  

When choosing a service model, consider which party should be responsible for the computing resource. Depending on your scenarion, you can decide how much shared management responsibility you want. The following displays a list of resources you manage and your service provider manages in each cloud service category.  


<img src="/images/3-shared-responsibility.png" alt="responsbilities" style="width: 531px;">  

#### Infrastructure as a Service (IaaS):  
- instant computing infrastructure, provisioned and managed over the Internet.   
- IaaS enables you to quickly scale resources to meet demand and only pay for what you use.  
- IaaS avoids the expense and complexity of buying and managing your own physical servers and other datacenter infrastructure.  
- Each resource is offered as a separate service component, and you rent the resource as long as you need it.  