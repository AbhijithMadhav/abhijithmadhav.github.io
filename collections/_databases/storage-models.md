---
title: Storage Models
excerpt: How is data organized in storage
---

Storage models refer to the way data is stored on the storage media

# Row oriented
Rows/Records are stored contiguously in the storage medium.
Typically relational OLTP databases do this.
This helps with query patterns that often seek to retrieve rows together.

Used in Oracle, Postgres, MySQL

# Column oriented
Columns are stored together in the storage medium. Typically, OLAP databases do this.
This helps with query patterns that often seek to aggregate data in columns.

Used in Clickhouse

# Wide-column based
Used in Cassandra and Hbase