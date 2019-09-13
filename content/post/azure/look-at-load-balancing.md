---
title: "Look at Load Balancing"
date: 2019-09-13T15:33:45-04:00
draft: true
---

## Internal Load Balancer  

An Internal Load Blancer differes from a public Load Balancer; in that an internal Load Balancer directs traffic only to resources that are inside a virtual network or use a VPN to access Azure infrastructure, whereas a public load balancer maps the public IP address and port number of incoming traffic to the private IP address and port number of the VM, and vice versa for the response traffic from the VM.  

Azure infrastructure restricts access to the load-balanced frontend IP addresses of a virtual network. Frontend IP addresses and virtual networks are never directly exposed to an internet endpoint. Internal line-of-business aplications run in Azure and are accessed from within Azure or from on-premises resources.  

An internal LB enables the following types of load balancing:  

* **Within a virtual network**: Load balancing from VMs in the virtual network to a set of VMs that reside within the same virtual network.

* **For a cross-premises virtual network**: Load balacning from on-premises computers to a set of VMs that reside within the same virtual network.  

* **For multi-tier applications**: Load balancing for internet-facing multi-tier applications where the backend tiers are not internet-facing. The backend tiers require traffic load-balancing from the internet-facing tier.  

* **For line-of-business applications**: Load balancing for line-of-business applications that are hosted in Azure without additional load balancer hardware or software. This scenario includes on-premises servers that are in the set of computers whose traffic is load-balanced.  

<img src="/images/ic744147.png" alt="public-and-internal-lbs" style="width: 642px;">  

*Above: load balancing multi-tier applications using both public and internal load balancers.*  

## Public Load Balancer  

A public Load Balancer maps the public IP address and port number of incoming traffic to the private IP address and port number of the VM, and vice versa for the response traffic from the VM. By applying load-balancing rules, you can distribute specific types of traffic across multiple VMs or services. For example, you can spread the load of web request traffic across multiple web servers.  

The following figure shows a load-balanced endpoint for web traffic that is shared among three VMs for the public and TCP port 80. These three VMs are in a load-balanced set.  

<img src="/images/ic727496.png" alt="public-lb-only" style="width: 527px;">  

*Above: Load balancing web traffic by using a public Load Balancer.*  

When internet clients send webpage requests to the public IP address of a web app on TCP port 80, Azure Load Balancer distributes the requests across the three VMs in the load-balanced set.  

By default, Azure Load Balancer distributes network traffic equally among multiple VM instances.  