---
title: "Azure Scaling"
date: 2019-09-04T12:15:15-04:00
category: "azure"
---

[source](https://docs.microsoft.com/en-us/learn/modules/design-for-performance-and-scalability-in-azure/2-scaling-up-and-scaling-out)  

# Scaling Up/Out  

We'll define *scaling up/down* and *scaling out/in*, cover some ways Azure can improve scaling capabilities, and look at how serverless and container technologies can improve your architecure's ability to scale.  

## Scaling. What is it?  

*Scaling* is the process of managing your resources to help your application meet a set of performance standards.  

When there is too many resources serving users, the resources will not be used efficiently and money will be wasted. In contrast, when they are too few available resources, the performance of the application could be impacted. The goal is to meet set performance requirements while optimizing for cost.  

*"Resources"* refers to anything we need to manage to run our applications. Memory and CPU for virtual machines are the most obvious resources, but some Azure services might require you to consider bandwidth or abstractions, like Azure Cosmos DB Request Units.  

In the real world, the demands of applications change over time, so the right amount of resources you'll need can be harder to predict.  

Ideally, you will want to provision the right amount of resources to meet the demand and adjust as demand changes.  

Easy scaling is a key benefit of cloud computing. Most Azure resources let you easily add or remove resources as demand changes, and many services have automated options so they monitor demand and adjust for you. In other words, autoscaling, lets you set thresholds for the minimum and maximum level of instances that should be available, and will add or remove instances based upon a performance metric (i.e. CPU utilization).  

## Scaling up, or scaling down?  

**Scaling up:** process of increasing the capicity of a given instance.  

For example, vCPU 1 and RAM 3.5GB to vCPU 2 and RAM 7GB; in order to provide more processing capacity.  

**Scaling down:** process where we lower the capacity of a given instance, which reduces capacity and cost.  

What scaling up or down means in the context of Azure resources:  
* For Azure virtual machines, you scale based upon a virtual machine size. That size has a certain amount of vCPUs, RAM, and local storage associated to it.  
    - For example, we could scale up from Standard_D2s_v3 (2 vCPU and 8GB RAM) to a Standard_4s_v3 (4 vCPU and 16 GB RAM).  

* Azure SQL Database is a platform as a service (PaaS) implementation of Microsoft SQL Server. You can scale up a database based upon the number of database transaction units (DTUs) or vCPUs. DTUs are an abstraction of underlying resources and area blend of CPU, IO, and memory.  
    - For instance, you could scale your Azure SQL Database from a size of P2 with 250 DTUs up to a P4 with 500 DTUs; in order to give the database more throughput and capacity.  

* Azure App Service is a PaaS website-hosting service on Azure. Websites run on a virtual server farm, an *App Service Plan*. YOu can scale the App Service plan up or down between tiers and have the capacity options within tiers.  
    - For example, an S1 App Service plan has 1 vCPU and 1.7 GB of RAM per instance. We could scale up to an S2 App Service plan, which has 2 vCPUs and 3 GB of RAM per instance.  

To have these capabilities in an on-premises environment, you would typically have to wait for procurment of the additional hardware and installation, before you can even start using the new level of scale. In a cloud computing platform like Azure, the physical resources are already deplyoed and available for you; all you need to do is select the alternate level of scale.  

*you may need to consider the impact of scaling up in your solution, depending on the cloud services that you have chosen.*  
For example, if you choose to scale up Azure SQL Database, the service deals with scaling up individual nodes and continues the operation of your service. Changing the service tier and/or performance level of a database creates a replica of the original database a tthe new performance level, and then switches connections over to the replica. No data is lost during this process, and a brief interruption (typically less than four seconds) when the service switches over to the replica.  

Alternatively, if you choose to scale up or down a virtual machine, you do so by selecting a different instance size. In most cases this requires a restart of the VM, so it's best to have the expectation that a reboot will be required and you'll need to account for when performing this activity.  

Finally, you should always look for palces where scaling down is an option. If your application can provide adequate performance at lower price tier, your Azure bill could be significantly reduced.  

## Scaling out or in?  

We've talked about how scaling up and down adjusts the amount of resources a single instance has available. Scaling out and in adjusts the total number instances.  

**Scaling out:** process of adding more instances to support the load of your solution. For example, if your front end were hosted on virtual machines, we could increase the number of virtual machines if the level of load were increased.  

**Scaling in:** process of removing instances that are no longer needed to support the load of your solution. If the website front ends have low usage, we may want to lower the number of instances to save cost.  

Here's what that means in the context of Azure resources:  

* For the infrastructure layer, you would likely use virtual machine scale sets to automate the addition and removal of extra instances.  
    - Virtual machine scale sets let you create and manage a group of identical, load balanced VMs.  
    - The number of VM instances can automatically increase or decrease in response to demand or a defined schedule.  

* In an Azure SQL Database implementation, you could share the load across database instances by sharding. *Sharding* (not sharting) is a technique to distribute large amounts of identically structured data across a number of independent databases.  

* In Azure App Service, the App Service plan is the virtual web server farm hosting your application. Scaling out in this way means that you are increasing the number of virtual machines in the farm. As with virtual machine scale sets, the number of instances can be automatically raised or lowered in response to certain metrics or a schedule.  

Scaling out is typically easily performed with the Azure Portal, command-line tools, or Resource Manager templates, and in most cases is semaless to the end user.  

## Auotscaling  

You can configure some of these services to use a feature called autoscale.  

* no longer have to worry about scaling services manually  
* instead, set a minimum/maximum threshold of instances and scale based upon specific metrics (queue length, CPU utilization) or schedules (weekdays between 5:00 PM and 7:00 PM).  

The following shows how the autoscale feature manages instances to handle the load.  

<img src="/images/2-autoscale.png" alt="autoscale" style="width: 628px;">  

## Considerations when scaling in and out  

When scaling out, the startup time of your application can impact how quickly your applciation can scale. If your web app takes two minutes to start up and be available for users, that means each of your instances will take two minutes until they are avaialble to your users. Therfore, you will want to take this startup time into consideration when determining how fast you want to scale.  

You will also need to think about how your application handles state. When the application scales in, any state stored on the machine is no longer available. If a user connects to an ainstance that doesn't have it's state, it could force them to sign in or re-select data, leading to a poor user experience.  

A common pattern is to externalize state to another service like Redis Cache or SQL Database; in order to make your web servers stateless. If your web front ends are stateless, you don't need to worry about which individual instances are available. They are all doing the same job and are deployed in the same way.  

## Throttling  

So we know that the load on an application can vary over time. While we could use autoscaling to add capacity, we could also use a throttling mechanism to limit the number of requests from a source. We can safegaurd performance limits by putting known limits into place at the application level, preventing the application from breaking. Throttling is most frequently used in applications exposing API endpoints.  

Once the application has identified that it would breach a limit, throttling could begin an densure the overall system SLA isn't breached. For example, if we exposed an API for customers to get data, we could limit the number of requests to 100 per minute. If any single customer exceeded thsi limit, we could respond with an HTTP 429 status code, including the wait time before another request can be successfully submitted.  

## Serverless  

Serveless computing provides a cloud-hosted execution environment that runs your apps; however, the underlying environment is completely abstracted. You create an instance of the service, and you add your code; no infrastructure management or maintenance is required, or even allowed.  

You cnofigure your serverless apps to respond to events. This could be a REST endpoint, a timer, or a message received from another Azure service. THe serverless app runs only when it's triggered by an event.  

Infrastructure is not your responsibility. Scaling and preformance are handled automatically, and you are billed only for the exact resources you use. There's no need to even reserve capacity. Azure Functions, Azure Container Instances, and Logic Apps are all examples of serverless computing available on Azure.  

