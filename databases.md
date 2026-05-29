# AWS Databases Reference Guide
## Core Cloud Architecture & SAA-C03 Objective Summary

This reference guide outlines the core technical mechanics, storage layers, scaling pathways, and architectural trade-offs of AWS database services required for the AWS Certified Solutions Architect – Associate (SAA-C03) exam.

---

## 1. Relational vs. Non-Relational Frameworks

Selecting a database backend depends entirely on data structural layout, schema rigidity, transaction overhead, and expected data access patterns.

| Database Class | AWS Implementation | Primary Data Engine | Core Architectural Characteristics |
| :--- | :--- | :--- | :--- |
| **Relational (OLTP)** | **Amazon RDS** / **Aurora** | MySQL, PostgreSQL, MariaDB, Oracle, SQL Server | Structured tables, rigid schemas, declarative SQL, complex joins, strict ACID compliance. |
| **Non-Relational (NoSQL)** | **Amazon DynamoDB** | Key-Value / Document (JSON structures) | Horizontal partitioning, schema-less, single-digit millisecond latency at arbitrary scale. |
| **In-Memory Store** | **Amazon ElastiCache** | Redis, Memcached | Sub-millisecond latency, volatile or semi-persistent RAM storage, query caching. |
| **Analytical (OLAP)** | **Amazon Redshift** | Columnar Storage Engine | Massively Parallel Processing (MPP), petabyte-scale data warehousing, complex aggregation. |
| **Graph Database** | **Amazon Neptune** | W3C RDF / Property Graphs | Optimized for highly connected datasets, fast traversal of relationship edges. |

---

## 2. Amazon RDS (Relational Database Service) Architecture

Amazon RDS automates infrastructure provisioning, patching, and backups for standard relational engines. Designing an RDS deployment requires balancing high availability against read throughput performance.

### High Availability: Multi-AZ Deployments
* **Synchronization:** Processes data modifications by executing **synchronous replication** from the primary database instance to a hot standby instance deployed inside an entirely isolated Availability Zone.
* **Operational State:** The standby database instance remains fully passive. It cannot accept direct application read or write connections.
* **Failover Mechanics:** If the primary instance fails, AWS alters the canonical DNS endpoint string to point to the standby instance. This failover is automated, minimizes recovery time, and protects against data loss.
* **Primary Objective:** High availability, disaster recovery, and infrastructure fault tolerance.

### Scaling Throughput: Read Replicas
* **Synchronization:** Processes data modifications using **asynchronous replication** originating from the primary write instance.
* **Operational State:** Read replicas are active, standalone endpoints capable of servicing read-only application queries. They can be deployed within the local AZ, across different AZs, or across distinct AWS Regions.
* **Failover Mechanics:** Read replicas do not participate in automated infrastructure failover. If the primary instance fails, a replica must be manually promoted to a standalone master database.
* **Primary Objective:** Horizontal scaling of read-heavy operational workloads and offloading business intelligence analytics.

---

## 3. Amazon Aurora Architecture

Amazon Aurora is an enterprise-class, cloud-native relational engine built on a decoupled, virtualized storage layer.