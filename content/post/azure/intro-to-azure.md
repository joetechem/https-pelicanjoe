---
title: "Intro to Azure"
date: 2019-03-26T12:05:16-04:00
---  

Abstract: Basic overview of Azure.  

### Contents:  

* [Regions](#Regions)  
* [Resiliency & Availability](#Resiliency-&-Availability)
* [Compute](#Compute)  
* [Storage](#Storage)  

## Regions  

Global Footprint: 36 generally available Regions, with 8 more coming soon.  

## Resiliency & Availability  

#### Single VM  
* Not resilient to failure  

#### Availability Set  
* **Portects against local hardware failures**  
* Resilient to failure  
    - consists of two or more forte domains that share a common power source, network switch  
    - enables you to protect against localized hardware failures within an Azure Region.  
* VMs in an Availability Set are distributed across the forte domains  
    - If there is a hardware failure in one domain, network traffic will be routed to *healthy" forte domains  
    - So, this help protect against failure within the data center  

#### Availability Zones  
* **Protects against data center failures**  
* For an extra level of availability, and to help protect against specific data center failures, there is **Availability Zones** --different data centers within a geographical area.  

#### Paired Regions (Regional Pairs)  
* **Protects against regional outage**  
    - You can deploy an application across multiple Regions
    - And you can use Azure Traffic Manager to distribute Internet traffic into those different Regions  
* Each Azure Region is *paired* with another single Region
    - together, these Regions form a **Regional Pair**  
    - The two regions paired together within a Region Pair, are located within the same geography; in order to meet data residency requirements for tax and law purposes.  
* Regional Pairs enable you to build resilient architectures that help protect against regional failures  

**Note:** When you design multi-region architectures for applications; take into account things such as network latency across regions. --if you're replicating a database to enable failover, it is a good idea to do synchronous data replication within a particular Region WITH asynchronous data replication across Regions for performance reasons.  

Some services (such as Azure Cosmos DB), have built-in support for multi-region deployments.  

## Subscriptions  

Think of Subscriptions as an account within Micorsoft Azure.  

* **Subscription:** An agreement with Microsoft to use one or more cloud platforms or services.  
    - Basically, this is an agreement between yourself and Microsoft to pay for services on a consumption-based model.  
* Some organizations can have multiple Azure subscriptions  
* Subscriptions integrate with **Azure Active Directory**  
    - subscriptions build a trust relationship with Azure Active Directory  
    - e.g. your subscription trusts the directory to authenticate Users, Services, and Devices against it.  
    - **Multiple Subscriptions can trust the same Directory tenant, but each Subscription can only trust ONE Directory.**  
    - All Azure resources reside within a Subscription,  
    - So, when you deploy resources within Azure, you decide which Subscription you want those resources to live within  

## Azure Resource Manager  

Is the underlying service for deploying and managing resources in Azure.  

Let's you want to deploy, manage, and monitor reources as a group --Azure Resource Manager is how you achieve this.  

#### KEY TERMINOLOGY  

* Resources  
    - Manageable items available for Azure  
    - i.e. VMs, Storage Accounts, Web Apps, Databases, Virtual Networks, and many more.  
* Resource Groups  
    - A container which holds related reources for an Azure solution  
    - Best Practice: If you're going to do deployments for a certain application together, then the resources that make up the application should reside in the the same Resource Group  
* Resource Providers  
    - A service that supplies the resources you can deploy and manage for a Resource Manager  
        - Each of the Azure product groups, and the teams making these services, create a Resource Provider to manage those services.  
        - i.e. `Microsoft.Compute` (supplies the machine resources), `Microsoft.Storage` (supplies the storage account resources); as developers and users of Azure, we can leverage those Resource Providers to deploy, manage, and update those resources within the Azure cloud.  
* Resource Template Manager (**Infrastructure as Code**)
    - JSON files which defines one or more resources to deploy to a Resource Group (i.e. templating out the infrasructure to the cloud as code, in a JSON document)  
    - Defines the dependencies between the deployed resources, so it knows which of those relies on the others as well)  
    - These templates can be used to deploy resources in a conistent and repeatable fashion  

### Resource Template Manager is Similar to AWS CloudFormation, but...  
* Azure Resource Manager is the underlying service for deploying and managing Azure resources  

* AWS CloudFormation is purely a templating engine and state-mgmt service, so if you make changes outside of the CloudFormation, it is not reflected back in the template.  
    - Whereas, in Azure Resource Template Manager,  you can go to any resources you've deployed within the Azure Portal, and you can generate a template from them at a specific point.  

***  

# Compute  

Compute Services Azure Offers:  
* **Virtual Machines**  
* **App Service**  
* **Functions**  
* **Batch**  
* **Container Service**  
* **Container Instances**  
* **Service Fabric**  
* **Cloud Services** *(Classic)*    

Overview of each Azure Compute service:  

## Virtual Machines (Think Amazon EC2)  
* Windows and Linux machines on-demand  
* Endorsed Linux Distros:  
    - CentOS  
    - CoreOS  
    - Debian  
    - Oracle Linux  
    - RHEL  
    - SUSE LES  
    - openSUSE  
    - Ubuntu  
* You can bring your own distribution to virtual machines, but they will not be *supported*  

* 6 Types of Virtual Machines with 28 Families  
* Set amount of vCPUs, Memory, and Temporary Storage  
* Can attach disks  
    - for any persistent data, store on disks  
* Granular Billing  
* Per Minute Billing  
* Reserved VM Instances gives a sginificant discount  

## App Service  

* Platform as a Service  
    - Just upload your code  
* Completely Managed Service  
    - All you need to worry about is your application code  

* Supported Languages:  
    - .NET  
    - .NET Core  
    - Java  
    - Ruby  
    - Node.JS  
    - PHP  
    - Python  
* Varied number of *types* of applications in App Services  
    - Web Apps + APIs + Mobile Backends + Containers  

* Windows & Linux  
    - choose what you want to run *under the hood*  

* Various *App Service Plans* 
    - from Free to Isolated Environments  
    
## Azure Functions  

* Serverless Compute - Run code on demand  
* Function as a Service (FaaS)  
    - Google: Cloud Functions  
    - AWS: Lambda  
    
* Execute code in response to events or Triggers  
* Only pay when your code is executed (cost-effective)  
* C#, F#, JavaScript, Java (Preview)  
* Part of App Service and can run within an App Service Plan  
* First 1 million executions/mth are free  

## Batch  

* Managed Service for batch processing jobs  
* Used for running large scale parallel and HPC workloads efficiently  
* Scale processes to as many compute cores as required  
* Supports both Windows and Linux compute nodes  
* Batch is free.  
    - Just pay for the resurces your job consumes  
    
## Container Service  

* Managed Kubernetes Container Orchestration (AKS = Azure Kubernetes Service)  
    - Kubernetes: open source container orchestration developed at Google for orchestrating containers in a cluster env for large-scale microservice applications  
    
* Auto upgrades and patching (managed operation)  
    - simply spin up a Kubernetes cluster with one command or in portal, then go ahead and deploy your containers here  
    
* Support for other orchestrators (but not managed)  
    - DC/OS, Un-managed Kubernetes, Docker  
* Pay only for the agent nodes, not the servers (cost-effective)  

* Similar to AWS EC2 Container Service or AWS Fargate  

## Container Instances  

* Containers as a Service (CaaS)  

So if you have a small application that runs in a container, and you want to make it available, you can use: azure container instances  
* Fast and Easy way to run a container in Azure  
* Useful for applications that can run in isolated containers  
* Containers get a public IP address  
* Can design the container spec yourself (CPU/RAM)  
* Supports both Windows and Linux based containers  
* Per-second billing ( as apposed to per-minute billing with VMs)  
    - If you have a simple app that you want to run, you can package it up as a conatiner and run it in Azure Container Instances  
* Not ideal for all use cases but definitely handy!  

## Service Fabric  

* Platform for running microservices and containers  
* Used by a lot of Azure and MS services  
    - Sype for Business, Cortana, CosmosDB, Dynamics 365  
* Can run anywhere, other clouds, on premises. SDK is identical.  
* Supports STATEFUL and STATELESS micro services  
* Popular amongst the .NET community, but supports other languages and containers too  
* Utilizes high density container architecure for scale and performance  
    - you can have thousands of containers running on a small number of nodes  
    - efficient for performance and able to scale nicely  
    
## Cloud Services  

* Original PaaS Offering from Azure  
* Similar to App Service, but you can remote into the VMs (and install your own software too)  
* Web Roles - Websites  
* Worker Roles - Async processing  
* Recommended to use App Service instead of "Classic" Cloud Services  

***  

# Storage  
