## SQL vs NoSQL[^1]

| Aspect         | SQL (Relational DB)     | NoSQL (Non-relational DB)             |
| -------------- | ----------------------- | ------------------------------------- |
| Data model     | Tables (rows & columns) | Key-Value / Document / Column / Graph |
| Schema         | **Fixed schema**        | **Schema-less / flexible**            |
| Relationships  | Strong (JOINs, FK)      | Weak / application-managed            |
| Transactions   | **ACID**                | BASE (eventual consistency)           |
| Scalability    | Vertical (scale up)     | **Horizontal (scale out)**            |
| Consistency    | Strong                  | Eventual / tunable                    |
| Query language | SQL (standardized)      | DB-specific APIs                      |
| Performance    | Complex queries, joins  | High throughput, low latency          |
| Use cases      | Financial systems, ERP  | Big data, real-time apps              |

## ACID vs BASE (Very Important in Interviews)

### SQL → **ACID**

- **Atomicity** – all or nothing
- **Consistency** – valid state always
- **Isolation** – concurrent txns don’t interfere
- **Durability** – committed = permanent
👉 Best when **data correctness is critical**

### NoSQL → **BASE**

- **Basically Available**
- **Soft state**
- **Eventually consistent**
👉 Best when **availability & scale > immediate consistency**

## When to Use SQL
- Strong consistency is mandatory
- Complex queries & joins are required
- Data structure is stable
- Transactions matter

## When to Use NoSQL
- Schema evolves frequently
- Massive scale & traffic
- High write/read throughput
- Geo-distributed systems
---
# Indexing Strategies[^2]

### Why Indexing Matters
- Avoid full table scans
- Reduce I/O (disk → memory)
- Enable logarithmic lookup (B-Tree)
- Critical for **latency SLAs**
## Core Index Types (Most Important)

### 1. B-Tree Index (Default)
- Balanced tree → O(log n)
- Used by **Postgres, MySQL, Oracle**
### 2. Hash Index
**Best for:** Exact matches only
- O(1) lookup
- ❌ No range queries
- Limited usage (Postgres supports but rarely recommended)
### 3. Composite (Multi-Column) Index
**Best for:** Queries with multiple filters
Optimistic vs Pessimistic Locking

### 4. Covering Index
**Best for:** Read-heavy queries
### 5. Unique Index
**Best for:** Data integrity + performance
- Prevents duplicates
- Faster lookups
- Often backs `PRIMARY KEY`
### 6. Partial Index

**Index only subset of rows**
- Smaller index
- Faster scans
- Great for soft deletes
### 7. Functional Index
**Index on computed 

### 8. Full-Text Index
**Text search**

[^1]: > _“SQL databases prioritize consistency and relational integrity, while NoSQL databases prioritize scalability, availability, and schema flexibility.”_

[^2]: “Indexing is query-driven optimization; the goal is to minimize data scanned, not to index everything.”
