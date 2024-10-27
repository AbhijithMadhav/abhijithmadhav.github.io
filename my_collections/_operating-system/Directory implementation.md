---
title: Directory structure implementation
---

As a linear list

-   Implementation is simple
-   Performance wise no good as we require linear time to search through
    the entries
-   Can use a sorted list.

As a hash table

-   Major disadvantage is the general fixed size and the dependence of
    the hash function on that size. Resizing in this case may require
    reinsertion of all entries.
-   Use of separate chaining hash tables helps in this scenario
