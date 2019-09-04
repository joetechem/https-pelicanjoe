---
title: "Optimize Network Performance"
date: 2019-09-04T15:37:43-04:00
---

[source](https://docs.microsoft.com/en-us/learn/modules/design-for-performance-and-scalability-in-azure/3-optimize-network-performance)  

In complex architectures with many different services, minimizing the latency at each hop can have a huge impact on the overall performance. Here, we'll talk about the importance of network latency and how to reduce it within an architecture.  

## Importance of Network Latency  

Latency is the measurement of delay. Network latency is the time needed to get from a source to a destination across some network infrastructure. This time period is commonly known as a round-trip delay, or the time taken to get from the source to destination and back again.  

In a traditional datacenter environment, latency may be minimal since resources often share the same location and a common set of infrastructure. The time taken to get from source to destination is lower when resources are physically close together.  

In comparision, a cloud environment is built for scale. Cloud-hosted resources my not be in the same rack, datacenter, or even region. This distributed approach can have an impact on the round-trip time of your netowrk communications. While all Azure regions are interconnected by a high-speed fiber backbone, the speed of light is still a physical limitation. Calls between services in different physical locations will still have network latency directly correlated to the distance between them.  

On top of this, a *chattier* application requires more round trips. Each round trip comes with a latency tax, with each round trip adding to the overall latency. The following illustration shows how the latency perceived by the user is the combination of the roundtrips required to service the request.  

<img src="/images/3-networklatency.png" alt="networklatency" style="width: 348px;">  

Next, we'll look at how to improve performance between Azure resources and from your end users to your Azure resources.  

## Latency between Azure resources  

Imagine that a healthcare company is piloting a new patient booking system running on several web servers and a database in the West Europe Azure region. This architecture minimizes the data time on the wire as resources are co-located inside an Azure region.  

Suppose the pilot of the system went well and has been expanded to users in Australia. Users here will incur the round-trip time to the resources in West Europe to view the website, and their end-user experience will be poor due to network latency.  

The healthcare company team decides to host another front-end instance in the Australia East region to reduce user latency. While this design helps to reduce the time for the web server to return content to end users, their latency is still significant due the communication between the front-end web servers in Australia East and the database in West Europe.  

A few ways to reduce the remaining latency:  

* Create a read-replica of the database in Australia East.  
    - allowing reads to perform well, but writes would still incur latency. Azure SQL Database geo-replication allows for read-replicas.  

* Sync your data between regions with Azure SQL Data Sync.  

* Use a globally distributed database such as Azure Cosmos DB. This would allow both reads and writes to occur regardless of location, but may require changes to the way your application stores and references data.  

* Use caching technology such as Azure Cache for Redis to minimize high-latency calls to remote databases for frequently accessed data.  

The goal here is to minimize the network latency between each layer of the application. How this is solved depends on your application and data architecture, but Azure provides mecahnisms to solve this on several services.  

## Latency between users and Azure resources  

We've looked at latency between our Azure resources, but we should also consider the latency between users and our cloud application. We're looking to optimize delivery of the front end-user interface to ur users. Let's take a look at some ways to improve the network performance between end users and the application.  

### Use a DNS load balancer for endpoint path optimization  

In the healthcare company example, we saw that the team created an additional web front-end node in Australia East. However, end users have to explicitly specify which front-end endpoint they want to use. As the designer of a soluton, the healthcare company wants to make the experience as smooth as possible for their users.  

Azure Traffic Manager could help. Traffic Manager is a DNS-based load balancer that enables you to distribute traffic within and across Azure regions. Rather than having the user browse to a specific instance of our web front end, Traffic Manager can route users based upon a set of characteristics:  

* **Priority** - You specify an ordered list of front-end instances. If the one with the highest priority is unavailable, Traffic Manager will route the user to the next available instance.  

* **Weighted** - You would set a weight against each front-end instance. Traffic Manager then distributes traffic according to those defined ratios.  

* **Performance** - Traffic Manager routes users to the closest front-end instance based on network latency.  

* **Geographic** - You could set up geographical regions for front-end deployments, routing your users based upon data sovereignty manadates or localization content.  

Traffic Manager profiles can also be nested. You could first route your users across different geographies (for example, Europe and Australia) using geographic routing and then route them to local fornt-end deployments using the performance routing method.  

Consider that the same heatlhcare company has deployed a web front end in West Europe and Australia. Assume they have deployed Azure SQL Database with their primary deployment in West Europe, and a read replica in Australia East. Let's also assume the applciation can connect to the local SQL instance for read queries.  

The team deploys a Traffic Manager instance in performance mode and adds the two front-end instances as Traffic Manager profiles. As an end user, you navigate to a custom domain name (for example, healthcarecompany.com) which routes to Traffic Manager.  
