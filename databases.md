# AWS Databases Study Notes (Adrian Cantrill Style)
## SAA-C03 Exam Preparation Cheat Sheet

In Adrian Cantrill's architectural philosophy, you must never treat databases as a single generic entity. You must understand the exact mechanics of how data is stored, replicated, and accessed.

---

## 1. Database Types & The "Right Tool for the Job"

The SAA-C03 exam will present a business problem and expect you to select the exact database technology based on access patterns and data structures.

| Database Category | AWS Service | Data Structure | Ideal Workload / Key Terms |
| :--- | :--- | :--- | :--- |
| **Relational (OLTP)** | **Amazon RDS** / **Aurora** | Tables (Rows & Columns), Strict Schema, SQL, Joins | Complex transactional queries, ERP, CRM, traditional web apps. |
| **Non-Relational (NoSQL)** | **Amazon DynamoDB** | Key-Value / Document, Schema-less, JSON | Massive scale, predictable single-digit millisecond latency, simple access patterns. |
| **In-Memory Cache** | **Amazon ElastiCache** | Key-Value (Redis or Memcached) | Caching frequent read queries, session state storage, pub/sub. |
| **Data Warehouse (OLAP)** | **Amazon Redshift** | Columnar storage | Business Intelligence (BI), massive historical analytics, complex aggregations. |
| **Graph** | **Amazon Neptune** | Nodes, Edges, and Properties | Fraud detection graphs, social networks, recommendation engines. |

---

## 2. Amazon RDS (Relational Database Service)

RDS provides managed relational databases (MySQL, PostgreSQL, MariaDB, Oracle, SQL Server). Adrian stresses understanding the exact mechanical difference between **Multi-AZ Deployments** and **Read Replicas**.

### Multi-AZ Deployments (High Availability / DR)
* **Mechanics:** * Synchronously replicates data from a Primary DB instance to a Standby DB instance in a *different* Availability Zone.
    * The Standby instance is completely passive (you cannot connect to it or run queries against it).
* **Failover:** If the primary instance dies (hardware failure, AZ outage), AWS automatically updates the DNS endpoint to point to the Standby instance. Minimal downtime, zero data loss.
* **Exam Keyword:** "High Availability", "Disaster Recovery", "Business Continuity".

### Read Replicas (Scalability / Performance)
* **Mechanics:** * Asynchronously replicates data from the Primary DB instance to one or more Read Replicas.
    * Replicas can be inside the same AZ, a different AZ, or even a **Cross-Region** layout.
    * These instances are **active** and open for read-only queries.
* **Failover:** They are *not* designed for automatic failover. They can, however, be manually promoted to become their own standalone master database.
* **Exam Keyword:** "Read-heavy workload", "Scaling read performance", "Bi-reporting offloading".

---

## 3. Amazon Aurora (AWS-Native Relational)

Adrian spends significant time on Aurora because it represents a cloud-native redesign of relational databases.