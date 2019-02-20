---
title: "Lab Build a Custom VPC"
date: 2019-02-20T16:57:32-05:00
---

# Lab: Build a Custom VPC  

#### Abstract  

The goal of this lab is to create a custom Virtual Private Cloud (VPC) in Region us-east-1. The VPC will have a CIDR address range of /16 (IP address of 65,536). Within this VPC, two subnets in two separate Availability Zones (AZs) will be created, one *public* subnet (open to the Internet, or a route out to the Internet) and one *private* subnet (closed to traffic from the Internet).  

#### Objectives - Part One  

1. Launch a new VPC  
2. Create two Subnets  
3. Create an Internet Gateway (IGW)  
4. Attach the IGW to the VPC  
5. Create a new Route Table  
6. Associate Public Subnet with the new Route Table  
7. Enable auto-assign public IPv4 address for the Public Subnet  
8. Test Public/Private availability by provisioning two EC2 Instances, one in each subnet  
9. Connect to Public Instance in Public Subnet via SSH  

***  

## Build a Custom VPC - Part One  

### Step 1: Launch a new VPC  

* **Name:** April21VPC  
* **IPv4 CIDR block:** 10.0.0.0/16 (gets the biggest address range available for a VPC)  

As soon as the VPC is created, several things are automatically provisioned for the new VPC:  
* Default Route Table  
* Default Network ACL (Access Control List)  
* Default Security Group  


### Step 2: Create Two Subnets  

Create a public subnet in one AZ and a private subnet in another AZ, both inside the newly created custom VPC.  

#### Subnet Schema  

* Public  
    - **Name Tag:** 10.0.1.0 - us-east-1a  
    - **VPC:** April21VPC  
    - **AZ:** us-east-1a  
    - **CIDR block range:** 10.0.1.0/24 (256 IP addresses, Amazon reserves 5 of these)  

* Private    
    - **Name Tag:** 10.0.2.0 - us-east-1b   
    - **VPC:** April21VPC  
    - **AZ:** us-east-1b   
    - **CIDR block range:** 10.0.2.0/24 (256 IP addresses, Amazon reserves 5 of these)  

As of now, nothing is inside of them and both subnets are private. To establish some sort of Internet connectivity, you need to create and attach an Internet Gateway (IGW).  

### Step 3: Create an IGW  

* **IGW Name:** MyIGWapril21  

By default, this IGW is not attached to anything. In order to access the public subnet inside the VPC, it needs to have an Internet Gateway attached.  

You only need one IGW, as **you can only have one IGW attached per VPC.**  

You can test by creating another IGW and try attaching it the same VPC.  

### Step 4: Attach the IGW to the VPC  

Simply attach the IGW to the VPC by going to "Internet Gateways" under the VPC Dashboard in the AWS Console, select the IGW created. Under "Actions", attach to VPC.  

**Allowed only 1 IGW per VPC:** You cannot have multiple IGWs attached to a VPC.  

***  
#### Go to Route Tables  

As of now, the table's rules allow the subnets to talk to one another by default:  

#### Route Rules   

|   Destination    | Target  |  
|------------------|---------|  
| IPv4 10.0.0.0/16 | local   |  
| IPv6 ###'s ::/56 | local   |  

#### Subnet Associations  

As of now, there are no subnet associations.  

Both of the newly created subnets, *"have not been explicitly associated with any route tables and are therefore associated with the main route table."*  

**By default, everytime you create a subnet:**  
IT IS AUOTMATICALLY ASSOCIATED WITH THE MAIN ROUTE TABLE  

For this reason, you don't want the main route table (the one create by default after thecustom VPC was launched) to have a way out to the Internet. Because, everytime you provision a subnet, it is going to be Internet accessible.  

So, you need to create a new Route Table.  

***  

## Step 5: Create a new Route Table Inside the VPC  

*"A route table specifies how packets are forwarded between subnets within your VPC, the Internet, and your VPN connection."*  

* **Name:** MyInternetRouteOutPublic  
    - place it inside the custom VPC  

This is an entirely new Route Table to the default route table that was automatically provisoned after the VPC's creation.  

#### Add another route rule to the new Route Table to enable Internet access:  

|   Destination    | Target     |  
|------------------|------------|  
| IPv4 10.0.0.0/16 | local      |  
| IPv6 ###'s ::/56 | local      |  
| 0.0.0.0/0        | custom VPC |  

Now, every subnet associated with this route table will be a public subnet (Internet traffic is allowed)  

For consistency, add in a route out for IPv6 as well:  

|   Destination    | Target     |  
|------------------|------------|  
| IPv4 10.0.0.0/16 | local      |  
| IPv6 ###'s ::/56 | local      |  
| 0.0.0.0/0        | custom VPC |  
| ::/0             | custom VPC |  

These two new rules, give Internet accessibility to IPv4 and IPv6 for any subnet that is associated with this route table.  

## Step 6: Associate Public Subnet with the new Route Table  

Under Subnet Associations, associate the subnet, 10.0.1.0 - us-east-1a (the public subnet) with the new route table.  

As of now, *Auto-assign Public IP* under Subnets is by default, **No**. We want this as **Yes** for the public subnet; otherwise, you won't be able to access 

## Step 7: Enable auto-assign public IPv4 address for the Public Subnet  

Under Subnets, Subnet Actions for the public subnet, check *"Enable auto-assign public IPv4 address"*. So now, the public subnet will have an IPv4 address.  

## Step 8: Test Public/Private Availability  

by provisioning two EC2 Instances; an instance in each subnet.  

* Launch Instance **in Public Subnet**  
    - **AMI:** Amazon Linux  
    - **Instance Type:** t2micro  
    - Instance Details: 
        - **Network:** Custom VPC  
        - **Subnet:** 10.0.1.0 - use-east-1a  
    - Storage:  
        - **Type:** Gen Purpose SSD (GP2)  
            - **Size:** 8 GiB  
            - **IOPS:** 100/3000  
    - Security Group:  
        - Create a new security group  
            - Inbound Rules:  
                | Type | Protocol | Port | Source |  
                |------|----------|------|-------|  
                | SSH  | TCP (6)  | 22 | 0.0.0.0/0|  
                | HTTP | TCP (6)  | 80 | 0.0.0.0/0|  
                | HTTP | TCP (6)  | 80 | ::/0|  

Can only select default security group.  
So, if you made a security group in your default VPC, you cannot select that here.  

* **SECURITY GROUPS DO NOT SPAN VPCs**  
    - **The security group will only exist within the VPC you create**  

* Launch Instance **in Private Subnet**  
    - **AMI:** Amazon Linux  
    - **Instance Type:** t2micro  
    - Instance Details: 
        - **Network:** Custom VPC  
        - **Subnet:** 10.0.2.0 - use-east-1b 
    - Storage:  
        - **Type:** Gen Purpose SSD (GP2)  
            - **Size:** 8 GiB  
            - **IOPS:** 100/3000  
    - Security Group:  
        - Select existing security group (your default)  
            - not a newly created one, otherwise this instance would be public not private  

***  

## What Makes the Subnets Public vs Private  

* PublicWebServer (or name of the public EC2 instance, in the public subnet, in the custom VPC)  
    - has a public IPv4 address  
* PrivateWebServer (or name of the private EC2 isntance, in the private subnet, in the custom VPC)  
    - has no public IP address, for the auto-assign public IP address option is disabled (the subnet's default setting).  
    - so there is no way to connect to this instance (yet).  

***  
## Step 9: Connect to Public Instance in Public Subnet via SSH  

To do this via the public IP address for the instance that's inside the public subnet.  

As of now, these two instances cannot talk to eachother, for they are behind two separate Security Groups, in two separate AZ's, and have not allowed permissions for them to talk to eachother.  

#### Part One Summary:  
* Attached an IGW to the VPC  
* Created a new Route Table: the public route table  
    - Because we didn't want our default route table, which automatically associates any newly created subnet to the default (or main route table), giving it internet accessibility  
    - The new route table has a RouteOut to the Internet with 0.0.0.0/0 pointing to the IGW  
* Left the Network ACL alone, for now  
* Created two Security Groups in VPC:  
    - New security group made inside Public subnet  
    - Default security placed inside Private subnet  
* Provisioned two instances in both subnets  

***  

# Build a Custom VPC - Part Two    

## Network Address Translation (NAT)  

With the EC2 instance that is sitting in the private subnet, we'll try to access it. Essentially will be the database server.  

**Note**: After you stop an instance, then start it up again, the IP address is different than before.  

As of now, the Private EC2 instance is not internet-accessible.  

## Step 1: Create New Security Group  

* Under Security Groups  
    - Create a new security group (for the private db instance)  
    - **Add Inbound Rules:**  
        - SSH (22)  
        - MySQL/Aurora (3306)  
        - HTTP (80)  
        - HTTPS (443)  
        - All ICMP - IPv4 (0 - 65535)  
            - this allows you to ping instances in your private subnet from the public subnet  
    - Make the source for all Inbound Rules as the CIDR address for the public subnet (10.0.1.0/24)  

## Step 2: Move the Private Instance Behind the New SG  

* Under EC2 Dashboard  
    - Change the Private server's Networking/Change Security Groups to the new Security Group  

**The Private Instance in the private subnet is now accessible**  

## Step 3: Ping the Private Instance from the Public Instance  

* SSH into Public IP Address  
* Ping the Private Instance from here  
    - `ping 10.0.2.#`  
* SSH into Private Instance from the Public Instance by using the private key file  
    - Though, in real life you would not do this  
    - Instead, you would use a Bastion Host, a gateway between your public servers and private servers  

Whilst SSH'd into the private instance, you can `yum update -y`; however, it will time out. Because **the private instance has no route out to the internet.**  

# Network Address Translation (NAT)  

By default, any instance launched into a private subnet in an Amazon VPC is unable to communicate with the Internet through the IGW. To enable communication to the private subnet's instances, launch either NAT Instances (disable its source/destination checks) or NAT Gateways inside your public subnet (in the same VPC as the private subnet), then update your main/default VPC's route table with routes pointing to those NATs.  

## Step 4: Provide Internet Accesibility to the Private EC2 Instance via NAT Instance  

Though, NAT Instances are on their way out; better practice to use NAT Gateway  

* Launch NAT Instance **in Public Subnet** via EC2 Dashboard  
    - **AMI:** Amazon Linux AMI 2014.09.1 x86_64 VPC NAT HVM GP2    
    - **Instance Type:** t2micro  
    - Instance Details: 
        - **Network:** Custom VPC  
        - **Subnet:** 10.0.1.0 - use-east-1a  
    - Storage:  
        - **Type:** Gen Purpose SSD (GP2)  
            - **Size:** 8 GiB  
            - **IOPS:** 100/3000  
    - Security Group:  
        - Choose existing security group (one created earlier for the public instance)  
            - Inbound Rules:  
                | Type | Protocol | Port | Source |  
                |------|----------|------|-------|  
                | SSH  | TCP (6)  | 22 | 0.0.0.0/0|  
                | HTTP | TCP (6)  | 80 | 0.0.0.0/0|  
                | HTTP | TCP (6)  | 80 | ::/0|  

**Though we will need HTTPS, as we are going to download MySQL, and it does this over HTTPS (443).**  

* Under Security Groups  
    - Add Rule to SG for public instance (also now for the NAT Instance) to allow HTTPS (443) on Inbound  
    - Note: **Security Groups are STATEFUL, meaning responses in allowed inbound traffic are allowed to flow to outbound rules, and vice versa.**  


## Step 5: Disable Source/Destination Checks  

"Each EC2 instance performs source/destination checks by default. This means that the instance must be the source or destination of any traffic it sends or receives. **However, a NAT instance must be able to send and receive traffic when the source or destination is not itself. Therefore, you must disable *source*/destination checks on the NAT instance.**  

Don't need to do this for a NAT Gateway.  

## Step 6: Create a Route in the VPC  

Have a Route Out from your default Route Table (Main Table) via the NAT instance.  

* Under Route Tables,  
    - Add another route to the VPC's default route table:  

|   Destination    | Target       |  
|------------------|--------------|  
| IPv4 10.0.0.0/16 | local        |  
| IPv6 ###'s ::/56 | local        |  
| 0.0.0.0/0        | NAT instance |  

This adds a Route Out via NAT instance to the outside world.  

Now, log into public instance and then into the private instance (via SSH)  

`yum update -y` whilst inside the private instance, works now.  

* As of now, this setup with the NAT instance has a ton of bottlenecks:  
    - it is a single instance, in a single AZ, so it can only have so much network throughput because it is a t2micro.  
    - As the state gets larger and larger, you'll have to scale  
    - If this single instance/OS you are relying on crashes, you will lose internet access for all instances inside the private subnet  

Yes, you can have Auto Scaling groups, across multiple AZ's with multiple routes out to the internet, but that can get complicated quickly.  

NAT Gateway automates a lot of this, and gets rid of relying on single instances, in single AZ's.  

## Step 7: Terminate the NAT Instance  

Try `yum install mysql -y` inside the private instance. This does not work, because the NAT instance that provided the route out to the internet has "failed".  

## Step 8: Create a NAT Gateway  

* NAT Gateways  
    - operate on IPv4  
    - or Egress Only Internet Gateways (on IPv6)  

* Place NAT Gateway in the Public Subnet, in the Custom VPC  

You need an Elastic IP (EIP) address in order for the NAT Gateway to work.  

* Elastic IP Allocation  
    - Create New EIP  

#### Elastic IP Adresses (EIP)  
Are static, public IP addresses in the pool for the region that you can allocate to your account (pull from the pool) and release (return to the pool).  

EIPs allow you to maintain a set of IP addresses that remain fixed while the underlying infrastructure may change over time.  

Important points to understand about EIPs for the exam:  
* You must first allocate an EIP for use within a VPC and then assign it to an instance.  
* EIPs are specific to a region (that is, an EIP in one region cannot be assigned to an instance within a VPC in a different region).  
* There is a one-to-one relationship between network interfaces and EIPs.  
* You can move EIPs from one instance to another, either in the VPC or a different VPC within the same region.  
* EIPs remain associated with your AWS account until you explicitly release them.  
* There are charges for EIPs allocated to your account, even when they are not associated with a resource.  

## Step 9: Add Rule to the Main Route Table with Target as the NAT Gateway  

* Under Route Tables:  
    -  Get rid of the "Black Hole" (rule for the terminated NAT instance)  
    - Add an all-traffic rule (0.0.0.0/0) to routes with the NAT Gateway as its target  

To test, `yum install httpd -y` (apache) whilst connected into the private instance (connection made from public instance).  

#### NAT Gateways  

Highly available. NAT gateways in each AZ are implmented with redundancy. However, that AZ may fail for some reason.  

**So, it is a good idea to have multiple NAT Gateways across multiple AZ's;** just need a route out in your main route table to each gateway. Then, failover will happen automatically.  

Because of its built-in redundancy, NAT Gateways are not a single instance that you are totally relient on.  

Maintenance: AWS manages all maintenance for you.  

Bandwidth: NAT Gateways support up to 10Gbps.  

Bastion Servers: are not supported at the moment with NAT Gateways.  

Bastion Hosts are servers used to RDP or SSH into instances that are in your private subnets.  

NAT Gateways don't sit behind a Security Group like NAT instances do.  

***  

**Use Nat Gateways over NAT Instances**  

## EXAM TIPS  

#### NAT Instances  

* When creating a NAT instance, Disable Source/Destination Check on the instance.  

* NAT instances must be in a PUBLIC SUBNET  

* There must be a route out of the private subnet to the NAT instance, in order for this to work.  

* The amount of traffic that NAT instances can support depends on the instance size.  
    - If you are bottlenecking, increase the instance size.  

* You can create high availability using **Autoscaling Groups**, multiple subnets in different **AZs**, and a script to automate failover.  

* Behind a Security Group  

#### NAT Gateways  

* Preferred by the enterprise  

* Scale automatically up to 10Gbps  

* No need to patch  

* Not associated with security groups  

* Automatically assigned a public ip address  

* Remember to update your route tables and point them to your NAT Gateways  
    - having one NAT Gateway in one AZ is not good enough, you want them in multiple AZs, so you have more redundancy; in terms of AZ failure  

* No need to disable Source/Destination Checks  

* More secure than a NAT instance; you cannot SSH into a NAT Gateway  

* Amazon manages all the hoopla for you with NAT Gateways  

***  

# Network Access Control Lists (NACLs) vs. Security Groups  

Both can span subnets and Availability Zones  

**One Subnet can be associated with one Network ACL**  

A Network ACL is provides a second layer of security that acts as a firewall for controlling traffic in and our of a subnet.  

## Step 1: Create a New Network ACL  

Put it inside the Custom VPC. By default, all Inbound/Outbound traffic is denied.  

Not currently associated with any subnets.  

* Associate the public subnet with the new ACL  
    - You can only associate 1 subnet with 1 network ACL. 
    - Once we associated the public subnet with the new ACL, this subnet automatically got disassociated from the default ACL of the VPC, so now only the private subnet is asscociated with the default ACL.  

* Add Inbound rules for:  
    - Rule # 100: HTTP (80) for 0.0.0.0/0  
    - Rule # 200: HTTPS (443) for 0.0.0.0/0  
    - Rule # 300: SSH (22) for 0.0.0.0/0  
    - Rule # 400: RDP (3389) for 0.0.0.0/0  
    - NACLs are STATELESS, so this rule does not automatically flow to its Outbound rules  
* Replicate the Inbound rules for the Outbound rules  

#### Ephemeral Ports  

In practice, to cover the different types of clients that might initiate traffic to public-facing instances in your VPC, you can open ephemeral ports 1024-65535. However, you can also add rules to the ACL to deny traffic on any malicious ports within that range. Ensure that you place the DENY rules earlier in the table than the ALLOW rules that open the wide range of ephmeral ports.  

* Add a 5th rule for Inbound and Outbound:  
    - Rule # 500: Custom TCP (1024-65535) for 0.0.0.0/0  

* You can set rules to deny specific IP addresses (example over HTTP), but make sure to set this rule number to be sequentially BEFORE the *allow all-sources* rules, otherwise the rule will be ignored. Because rules are evaluated in order. The lower numbered rule will always take effect if you have a conflicting higher numbered rule.  
    - The blocked IP address can get around this, by using a VPN connection, for it changes the IP address it is coming from.  
    - You cannot block specific ports to a specific IP address with Security Groups  
    - So, Network ACLs provide much more granular control over your VPC than Security Groups, this is whey Network ACLs are used much more during production environments  

**One subnet can only be associated with one Network Access Control List (ACL)**  

**Rules are evaluated in numerical order**  

As soon as a rule is applied, takes effect immediately.  

## EXAM TIPS - NETWORK ACLs  

* Your VPC automatically comes with a default Network ACL, and by default it allows all outbound and inbound traffic.  

* When you can create custom Network ACLS. By default, each custom Network ACLS denies all inbound and outbound traffic until you add rules.  

* Each subnet in your VPC must be associated with a Network ACL. If you don't explicitly associate a subnet with a Network ACLS, the subnet is automatically associated with the default Network ACL.  

* You can associate a Network ACL with multiple subnets; however, a subnet can be assoicated with only one Network ACLS at a time. When you associate a Network ACL with a subnet, the previous association is removed.  
    - Network ACLs can span **Availability Zones**, whereas subnets cannot.  

* Network ACLs contain a numbered list of rules that is evaluated in order, starting with the lowest numbered rule.  

* Network ACLs have separate inbound and outbound traffic rules, and each rule can either allow or deny traffic.  

* Network ACLs are stateless; responses to allowed inbound traffic are subject to the rules for outbound traffic (and vice versa).  

* Remember to allow Ephemeral Ports on your Outbound Rules --outbound rules only.  

* You can block IP address using ACLs, not Security Groups  

***  

# Load Balancers & Custom VPCs  

Load Balancers ensure consistency for the end user by distributing traffic across multiple EC2 instances.  

## Step 1: Provision a Load Balanacer (Hypothetically)  

#### Three types of Load Balancers available:  
1. Application Load Balancer  
2. TCP  
3. Classic Load Balancer (not recommended)  

9 times out of ten, you would provision an Application Load Balancer. Unless you have ultra-high performance or you need static IP addresses with your application, then you'd use a TCP Load Balancer.  
#### Hypothetically provisioning an Application Load Balancer  

* When you choose a VPC to launch into:  
    - The load balancer routes traffic to the targets in these Availability Zones only. You can specify only one subnet per Availability Zone. **You must specify subnets from at least two Availability Zones to increase the availability of your load balancer.**  
    - and **the subnets in these AZ's must be public**  