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
        - allows customers to pay only per usage for the CPU cycles, storage, or bandwidth they consume  

* Why public cloud?  
    - can be deployed faster than on-premises infrastructures and with an almost infinitely scalable platform  
    - every employee of a company can use the same application from any office or branch using their device of choice via the internet  

Examples of why you'd use the public cloud:  

* **Service consumption through on-demand or subscription model:** The on-demand or subscription model allows you to pay for the portion of CPU, storage, and other resources that you use or reserve.  

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

## Software as a Service (SaaS):  

- Software as a service (SaaS) allows users to connect to and use cloud-based apps over the Internet.  
- Common examples are email, calendaring, and office tools such as Microsoft Office 365.  
- SaaS provides a complete software solution that you purchase on a pay-as-you-go basis from a cloud service provider.  
- You can rent the use of an application for your organization.  
- Your users connect to the service over the Internet, usually with a web browser.   
- All of the underlying infrastructure, middleware, app software, and app data are located in the service provider’s data center.  
- The service provider manages the hardware and software, and with the appropriate service agreement, will ensure the availability and the security of the app and your data as well.  
- SaaS allows your organization to get quickly up and running with an app at minimal upfront cost.  

Saas examples: Outlook, Hotmail, Yahoo! The email software is located on the service provider's network, and your messages are stored there too. You can access your email and stored messages from a web browser or any computer or Internet-connect device.  

**Common Scenarios:**  

* Let's imagine your healthcare company requires a customer relationship management (CRM) solution for its sales team. The team is global. You can use a SaaS CRM provider to quickly implement a solution to your organization's sales team.  

* For organizational use, you can rent productivity apps, such as email, collaboration, and calendaring; and sophisticated business applications such as customer relationship management (CRM), enterprise resource planning (ERP), and document management. You pay for the use of these apps by subscription or according to the level of use.  

### Advantages of SaaS  

**Gain access to sophisticated applications:** To provide SaaS apps to users, you don’t need to purchase, install, update, or maintain any hardware, middleware, or software. SaaS makes even sophisticated enterprise applications, such as ERP and CRM, affordable for organizations that lack the resources to buy, deploy, and manage the required infrastructure and software themselves. Pay only for what you use. You also save money because the SaaS service automatically scales up and down according to the level of usage.  

**Use free client software:** Users can run most SaaS apps directly from their web browser without needing to download and install any software. You don't need to purchase or deploy client software for your users.  

**Access app data from anywhere:** With data stored in the cloud, users can access their information from any Internet-connected computer or mobile device. And as app data is stored in the cloud, no data is lost if a user’s computer or device fails.  

In summary...  

Cloud computing resources are delivered using three different service models:  

1. Infrastructure-as-a-service (IaaS) provides instant computing infrastructure that you can provision and manage over the Internet.  

2. Platform as a service (PaaS) provides ready-made development and deployment environments that you can use to deliver your own cloud services.  

3. Software as a service (or SaaS) delivers applications over the Internet as a web-based service.  

***  

# Control Azure Services with CLI  

Looking at a scenario:  

You work at a company that develops Azure Web Apps. These are applications hosted in Azure, with all the benefits of automatically configured security, load balancing, management, and so on. You're currently testing a web app that generates sales forecasts, based on a range of inputs from different databases and other data sources. Your developers use Windows, linux, and Mac computers, and use GitHub for daily builds of the applications.  

As part of the testing, you want to compare app performance for different data sources, and for different types of data connections. You've noticed that when your development team uses the Azure Portal to create a new test instance of the app, they don't always use exactly the same parameters. You plan to solve this problem by using a set of standard deployment commands for each app test, which can be automated if required, and which will work in the same way across all the computers used by your software team.  

Here, we'll look out how to manage Azure resources using the Azure CLI.  

## What is Azure CLI?  

Azure CLI is a command-line program to connect to Azure and execute admin commands on Azure resources. It can be used interactively or scripted.  

For example, to restart a virtual machine (VM), you would use a command like this:  
```Powershell  
az vm restart -g MyResourceGroup -n MyVm  
```  

To automate repetitive tasks, you assemble the CLI commands into a shell script using the script syntax of your chosen shell and then execute the script.  

### Using Azure CLI in scripts  

If you want to use the Azure CLI commands in scripts, you need to be aware of any issues around the "shell" or environment used for running the script. For example, in a bash shell, the following syntax is used when setting variables:  

```Powershell  
variable="value"
variable=integer
```  

If you use a PowerShell environment for running Azure CLI scripts, you'll need to use this syntax for variables:  

```Powershell  
$variable="value"
$variable=integer
```  

## Working with Azure CLI  

The Azure CLI lets you type commands and execute them immediately from the command line. Recall that the overall goal in the software development example is to deploy new builds of a web app for testing. Let's talk about the sorts of tasks you can do with the Azure CLI.  

## What Azure resources can be managed using the Azure CLI?  

The Azure CLI lets you control nearly every aspect of every Azure resource. You can work with resource groups, storage, virtual machines, Azure Active Directory (Azure AD), containers, machine learning, and so on.  

Commands in the CLI are structured in *groups* and *subgroups*. Each group represents a service provided by Azure, and the subgroups divide commands for these services into logical groupings. For example, the `storage group` contains subgroups including **account**, **blob**, **storage**, and **queue**.  

So, how do you find the particular commands you need? One way is to use `az find`, the AI robot that uses the Azure documentation to tell you more about commands, the CLI and more.  

Example - find the most popular commands related to the word **blob**.  

```Powershell  
az find blob
```  

Example - Show me the most popular commands for an Azure CLI command group, such as `az vm`.  

```Powershell  
az find 'az vm'
```  

Example - Show me the most popular parameters and subcommands for an Azure CLI command.  

```Powershell  
az find 'az vm create'
```  

If you already know the name of the command you want, the `--help` argument for that command will get you more detailed information on the command, and for a command group, a list of the available subcommands. So, with our storage example, here's how you can get a list of the subgroups and commands for managing blob storage:  

```Powershell  
az storage blob --help
```  

## How to create an Azure Resource  

When creating a new Azure resource, there are typically three steps: connect to your Azure subscription, create the resource, and verify that creation was successful. The following illustration shows a high-level overview of the process.  

<img src="/images/4-create-resources-overview.png" alt="create-resources" style="width: 345px;">  

Each step corresponds to a different Azure CLI command.  

#### Connect:  
```Powershell  
az login
```  

The Azure CLI will typically launch your default browser to open the Azure sign-in page. If this doesn't work, follow the command-line instructions and enter an authorization code at https://aka.ms/devicelogin.  

After a successful sign in, you'll be connected to your Azure subscription.  

#### Create:  

You'll often need to create a new resource group before you create a new Azure service, so we'll use resource groups as an example to show how to create Azure resources from the CLI.

The Azure CLI `group create` command creates a resource group. You must specify a name and location. The name must be unique within your subscription. The location determines where the metadata for your resource group will be stored. You use strings like "West US", "North Europe", or "West India" to specify the location; alternatively, you can use single word equivalents, such as westus, northeurope, or westindia. The core syntax is:  

```Powershell  
az group create --name <name> --location <location>
```  

#### Verify:  

For many Azure resources, the Azure CLI provides a `list` subcommand to view resource details. For example, the Azure CLI `group list` command lists your Azure resource groups. This is useful here to verify whether creation of the resource group was successful:  

```Powershell  
az group list
#To get a more concise view, you can format the output as a simple table:
az group list --output table
```  

## Exercise - Create an Azure website using CLI  

Next, let's use the Azure CLI to create a resource group, and then to deploy a web app into this resource group.  

### Using a resource group  

When you're working with your own machine and Azure subscription, you'll need to first sign in to Azure using the `az login` command. However, signing in is unnecessary when you are using the browser-based Cloud Shell environment.  

Next, you would normally create a resource group for all your related Azure resources with an `az group create` command, but for this exercise the following resource group has been created for you: Learn-5d8bc07d-b377-4871-8890-aad8099c4445.  

1. Your first step in this exercise will be to create several variables that you will use in later commands.  

```bash  
export RESOURCE_GROUP=Learn-5d8bc07d-b377-4871-8890-aad8099c4445
export AZURE_REGION=[region]
export AZURE_APP_PLAN=popupappplan-$RANDOM
export AZURE_WEB_APP=popupwebapp-$RANDOM
```  

You can ask the Azure CLI to list all your resource groups in a table. There should just be one while you are in the free Azure sandbox.  

```Powershell  
az group list --output table
```  

As you do more Azure development, you can end up with several resource groups. If you have several items in the group list, you can filter the return values by adding a `--query option`. Try the following command:  

```Powershell  
az group list --query "[?name == '$RESOURCE_GROUP']"
```  

he query is formatted using **JMESPath**, which is a standard query language for JSON requests. You can learn more about this powerful filter language at http://jmespath.org/. We also cover queries in more depth in the **Manage VMs with the Azure CLI module**.  

### Steps to create a service plan  

When you run Web Apps using the Azure App Service, you pay for the Azure compute resources that are used by the app, and the resource costs depend on the App Service plan associated with your Web Apps. Service plans determine the region used for the app datacenter, number of VMs used, and pricing tier.  

Create an App Service plan to run your app. The following command specifies the free pricing tier, but you can run `az appservice plan create --help` to see the other pricing tiers.  

**The name of the app and plan must be unique in all of Azure. The variables that you created earlier will assign random values as suffixes to make sure they're unique. However, if you receive an error when you are creating any resources, you should run the commands listed earlier to reset all of the variables with new random values.**  

```Powershell  
az appservice plan create --name $AZURE_APP_PLAN --resource-group $RESOURCE_GROUP --location $AZURE_REGION --sku FREE
```  

***  

# Predict Costs and Optimize Spending for Azure  

* What will this solution cost this fiscal year?  

* Is there an alternate configuration you could use to save money?  

* Can you estimate how a change would impact your cost and performance without putting it into a production system?  

## Purchasing Options with Azure  

There are three main customer types on which the available purchasing options for Azure products and services are contingent:  

* **Enterprise**: Enterprsie customers sign an Enterprise Agreement with Azure that commits them to spend a negotiated amount on Azure services, which they typically pay annually. Enterprise customers also have access to customized Azure pricing.  

