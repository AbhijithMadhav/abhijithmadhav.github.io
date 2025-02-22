---
title: Storage Models
excerpt: How is data organized in storage?
---

Storage models refer to the way data is stored on the storage media.
This is also referred to as data orientation

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
A set of columns(column families) are stored together, row-wise. 
These columns can be indexed using a row identifier. 
The rows can have different columns.
The way this is achieved is by storing columns as key-value pairs within each row.

A row identifier can have multiple such column families.

Used in Cassandra, Hbase, DynamoDB.
