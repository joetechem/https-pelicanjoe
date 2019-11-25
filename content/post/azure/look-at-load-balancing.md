---
title: "Look at Load Balancing"
date: 2019-09-13T15:33:45-04:00
draft: true
---

# Azure Load Balancer  

* allows you to scale your applications and create high availability for your services.  

* Load Balancer supports inbound and outbound scenarios  

* provides low latency and high throughput  

* scales up to millions of flows for all TCP nad UDP applications  


* Load Balancer distributes new inbound flows that arrive on the LB's frontend to backend pool instances, according to rules and health probes.  

* a *public* Load Balancer can provide outbound connections for virtual machines inside your virtual network by translating their private Ip addresses to public IP addresses.  

* Azure Load Balancer is available in two SKUs: Basic and Standard. There are differences in scale, features, and pricing.  
    - Any scenario that's possible with Basic LB can also be created with Standard LB  

**You can use Azure LB to:**  

* Load-balance incoming internet traffic to your VMs. This configuration is known as a Public Load Balancer.  

* Load-balance traffic across VMs inside a virtual network. You can also reach a Load Balancer front end from an on-premises network in a hybrid scenario. Both scenarios use a configuration that is known as an Internal Load Balancer.  

* Port forward traffic to a specific port on specific VMs with inbound network address translation (NAT) rules.  

* Provide outbound connectivity for VMs inside your virtual network by using a public Load Balancer.  

<center>  

Azure provides a suite of fully managed load-balancing solutions for your scenarios. If you are looking for Transport Layer Security (TLS) protocol termination ("SSL offload") or per-HTTP/HTTPS request, application-layer processing, review Application Gateway. If you are looking for global DNS load balancing, review Traffic Manager. Your end-to-end scenarios might benefit from combining these solutions as needed.  

</center>  

## Fundamental Load Balancer Features  
LB provides the following fundamental capabilities for TCP and UDP apps:  

* **Load balancing**:  
    - create a load-balancing rule to distribute traffic that arrives at frontend to backend pool instances. Load Balancer uses a hash-based algorithm for distribution of inbound flows and rewrites the headers of flows to backend pool instances accordingly. A server is available to receive new flows when a health probe indicates a healthy backend endpoint.  

    - By default, Load Balancer uses a 5-tuple hash composed of source IP address, source port, destination IP address, destination port, and IP protocol number to map flows to available servers. You can choose to create affinity to a specific source IP address by opting into a 2- or 3-tuple hash for a given rule.  
    - All packets of the same packet flow arrive at the same instance behind the load-balanced front end. When the client initiates a new flow from the same source IP, the source port changes. As a result, the 5-tuple might cause the traffic to fgo to a different backend endpoint.  

<img src="/images/load-balancer-distribution.png" alt="public-and-internal-lbs" style="width: 734px;">  

## Load Balancer Resources  

An LB can exist either as a public LB or an internal LB.  

The LB resource's functions are expressed as a front end, a rule, a health probe, and a backend pool definition. You place VMs into the backend pool by specifying the backend pool from the VM.  

LB resources are objects within which you can express how Azure should program its multi-tenant infrastructure to achieve the scenario that you want to create. There is no direct relationship between Load Balancer resources and actual infrastructure. Creating a Load Blancer doesn't create an instance, and capacity is always available.  



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