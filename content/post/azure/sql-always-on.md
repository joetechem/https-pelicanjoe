---
title: "Sql Always On"
date: 2019-11-15T12:46:44-05:00
draft: true
---

# SQL Database High Availability with SQL Always On  

Deeply integrated with the Azure platform, Azure Sql Database features a built-in availability solution. It is dependent on Service Fabric or failure detection and recovery, on Azure Blob Storage for data protection, and on Availability Zones for higher fault tolerance. In addition, Azure SQL database leverages the Always On Availability Group technology from SQL Server for replication and failover. The combination of these technologies enables applications to fully realize the benefits of a mixed storage model and support the most demanding SLAs.  

The goal of the High Availability architecure in Azure SQL Database is to guarantee that your database is up and running 99.99% of time, without having to worry about the impact of maintenance operations and outages. Azure automatically handles critical servicing tasks, such as patching, backups, Windows and SQL upgrades, as well as unplanned events such as underlying hardware, software, or network failures. When the underlying SQL instance is patched or fails over, the downtime is not noticeable if you employ retry logic in your app.  

*[source](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-high-availability)*  

## Compnents  

* Availability Group  
* Storage Account for Cloud Witness  
* Storage Account for SQL backups  
* Cluster name and IP  
* Listener name and IP  
* Load Balancer to front end the SQL Always On  

# Configure Always On Availability Group Listeners  
*[source](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-ps-alwayson-int-listener)*  

* Create an internal laod balancer for SQL Server avaialbility groups using PowerShell cmdlets  
* Add additional IP addresses to a load balancer for more than one availability group  

**Availability Group Listener:** a virtual network name that clients connect to for database access.  
