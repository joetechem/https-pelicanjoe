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

## Infrastructure as a Service (IaaS):  
- instant computing infrastructure, provisioned and managed over the Internet.   
- IaaS enables you to quickly scale resources to meet demand and only pay for what you use.  
- IaaS avoids the expense and complexity of buying and managing your own physical servers and other datacenter infrastructure.  
- Each resource is offered as a separate service component, and you rent the resource as long as you need it.  

* As a result, IaaS is felxible.  
    - You can provision common infrastructure such as VMs, storage, virtual subnets, firewalls, and VPNs to build a solution.  
    - you don't need to manage physical servers and appliances.  

However, you are responsible for configuring and managing the components. For example, configuring firewalls, updating VM OS's, updating DBMS's, and runtimes.  

**Common Scenarios:**  

* Let's imagine your healthcare company has a need to run a special version of desktop software. The software is only supported on a specific version of an operating system and only one user license is required. You can create a virtual machine with the required software. The user can use a remote desktop connection to connect to the virtual machine to use the software.  

* Your development teams need several unique development environments. Through the development cycle, they need to test various versions of the product. The developers can provision environments when needed. When an environment is no longer needed, it can be easily deleted.  

other common scenarios:  

* **Website hosting:** If you want more control of hosting a website, running websites using IaaS may be a better option than traditional web hosting.  

* **Web apps:** IaaS provides all the infrastructure to support web apps, including storage, web and application servers, and networking resources. Organizations can quickly deploy web apps on IaaS and easily scale infrastructure up and down when demand for the apps is unpredictable.  

* **Storage, backup, and recovery:** Storage management can be complex, requiring a large capital investment and skilled staff to manage data and meet legal and compliance requirements. IaaS can help simplify planning, management, unpredictable demand, and steadily growing storage needs.  

* **High-performance computing:** If you have a workload that requires high-performance computing, you can run the workload in the cloud avoiding the up-front cost of the hardware and only pay for the usage when needed.  

* **Big data analysis:** If you have large data sets that contain potentially valuable patterns, trends, and associations, IaaS can provide the processing power to mine data sets to locate patterns.  

### Advantages of IaaS  

**Eliminates capital expense and reduces ongoing cost:** IaaS sidesteps the upfront expense of setting up and managing an on-site datacenter, making it an economical option for start-ups and businesses testing new ideas. As soon as you’ve decided to launch a new product or initiative, the necessary computing infrastructure can be ready in minutes or hours, rather than the days or weeks—and sometimes months—it could take to set up internally.  

**Improves business continuity and disaster recovery:** Achieving high availability, business continuity, and disaster recovery is expensive, since it requires a significant amount of technology and staff. But with the right service level agreement (SLA) in place, IaaS can reduce this cost and access applications and data as usual during a disaster or outage.  

**Respond quicker to shifting business conditions:** IaaS enables you to quickly scale up resources to accommodate spikes in demand for your application— during the holidays, for example—then scale resources back down again when activity decreases to save money. Because you don’t need to first set up the infrastructure before you can develop and deliver apps, you can get them to users faster with IaaS.  

**Increase stability, reliability, and supportability:** With IaaS, there’s no need to maintain and upgrade hardware or troubleshoot equipment problems. With the appropriate agreement in place, the service provider assures that your infrastructure is reliable and meets SLAs.  

## Platform as a Service (PaaS):  
- a complete development and deployment environment in the cloud  
- you purchase the resources from a cloud service provider on a pay-as-you-go basis and access them over a secure Internet connection  
- Like IaaS, PaaS includes infrstructure such as servers, storage, and networking.  
- In addition, it also includes middleware, development tools, and other services.  

PaaS supports the complete web application lifecycle: building, testing, deploying, managing, and updating.  

PaaS removes the need to manage software licenses, middleware, and infrastructure of the services.  

You manage the applications and services you develop, and the cloud service provider typically manages everything else.  

**Common Scenarios:** 

* Let's imagine your healthcare company needs a website to describe a product. Your developers want to use PHP. Using PaaS, your developers have the option to create a web app. The infrastructure details such as creating a virtual machine, installing a web server, and installing middleware are abstracted away. You don't need to care what operating system it runs on or what physical hardware is required. Your developers deploy the website files to the cloud and your website is available on the Internet.  

* Let's imagine another scenario. Your company needs a SQL database to support data analysts for a special project. You don't have infrastructure to accommodate the request. You can quickly provision a SQL Server in the cloud that meets the need of the project. The data analysts can connect to the server. The SQL Server database is provided as a service. Therefore, you don't worry about updates, security patches, or optimizing physical storage for reads and writes.  

other common scenarios:  

**Development framework:** PaaS provides a framework that developers can build upon to develop or customize cloud-based applications. Similar to the way you create an Excel macro, PaaS lets developers create applications using built-in software components. Cloud features such as scalability, high-availability, and multi-tenant capability are included, reducing the amount of coding that developers must do.  

**Analytics or business intelligence:** Analysis tools provided as a service allows you to analyze and mine data. Organizations can find insights and patterns to predict outcomes to improve forecasting, product design decisions, investment returns, and other business decisions.  

### Advantages of PaaS  

By delivering infrastructure as a service, PaaS has similar advantages as IaaS. But its additional features including middleware, development tools, and other business tools provide additional advantages:  

**Reduced development time:** PaaS development tools can reduce development time for new applications. Developers can use pre-coded application components built into the platform, such as workflow, directory services, security features, and search. Platform as a service components can give your development team new capabilities without you needing to add staff having the required skills.  

**Develop for multiple platforms:** Some service providers give you development options for multiple platforms, such as desktop, mobile devices, and browsers making cross-platform apps quicker and easier to develop.  

**Use sophisticated tools affordably:** A pay-as-you-go model makes it possible for individuals or organizations to use sophisticated development software and business intelligence and analytics tools that they could not afford to purchase outright.

**Support geographically distributed development teams:** Because the development environment is accessed over the Internet, development teams can work together on projects even when team members are at remote locations.  

**Efficiently manage the application lifecycle:** PaaS provides all of the capabilities that you need to support the complete web application lifecycle: building, testing, deploying, managing, and updating within the same integrated environment.  

