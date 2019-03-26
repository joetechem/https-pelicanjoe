---
title: "Intro to Azure"
date: 2019-03-26T12:05:16-04:00
---  

Abstract: Starting a basic overview of Azure, beginning with resiliency and availability.  

## Resiliency & Availability  

#### Single VM  
* Not resilient to failure  

#### Availability Set  
* Resilient to failure  
    - consists of two or more forte domains that share a common power source, network switch  
    - enables you to protect against localized hardware failures within an Azure Region.  
* VMs in an Availability Set are distributed across the forte domains  
    - If there is a hardware failure in one domain, network traffic will be routed to *healthy" forte domains  
    - So, this help protect against failure within the data center  

#### Availability Zones  
* For an extra level of availability, and to help protect against specific data center failures, there is **Availability Zones** --different data centers within a geographical area.  

#### Paired Regions (Regional Pairs)  
* Protects against regional outage  
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


