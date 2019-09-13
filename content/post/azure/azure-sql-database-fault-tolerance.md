---
title: "Azure Sql Database Fault Tolerance"
date: 2019-09-12T14:34:43-04:00
draft: true
---

In regards to building a Cloud Relational Database Mgmt System (RDBMS): To build a true RDBMS service, it is imperative to be fault-tolerant while ensuring that all atomicity, consistency, isolation and durability (ACID) characteristics of the service matched that of a SQL Server database.  Moreover, you'll want to provide elasticity and scale capabilities for customers to create and drop thousands of databases without any provisioning friction.  

#### A failure model can be simplified into two principles:  

1. Hardware and software failures are inevitable  
2. Operational staff make mistakes that lead to failures  

#### two major technologies that provide the foundation for the fault-tolerant databases:  

* Database Replication  
* Failure Detection & Failover  

## What is Database Fault-Tolerance?  

