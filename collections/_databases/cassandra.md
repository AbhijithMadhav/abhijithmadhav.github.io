---
title: Cassandra
excerpt : TODO
---

# Data modeling
- NoSQL
- No joins

# Replication
- Leaderless replication
  - Conflict resolution : LWW
- Read repair by clients
- Anti-Entropy process
- Quorum-based consistency 

# Partitioning
- Hash range, Consistent hashing
- md5
- Request routing : via nodes
  - Gossip protocol
- Secondary indexes: Local, partitioned by document

# Storage 
- Partitioned wide-column storage model
- LSM + SST 

# Coordinator

# Dynamo style db

# References
- Designing Data Intensive Applications - Martin Kleppman
- https://cassandra.apache.org/doc/latest/