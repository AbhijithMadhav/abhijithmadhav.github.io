---
title: Design considerations of a file system
---
Basic file-system algorithms

-   Reducing internal fragmentation in a clustered-linked-list
    allocation scheme

    In a [variation of the linked-list-allocation
    scheme](File%20Allocation%20Methods#Clustering), a cluster of
    blocks is allocated instead of a single block which results in a
    internal fragmentation. To reduce this, BSD Unix varies the cluster
    size as a file grows.

-   Fields of the FCB

    The types of data maintained in a FCB of a file merits some
    consideration. Fields like “last modified time” and “last access
    time” need to be updated often which results in the whole block
    containing this FCB to be written back to the disk(blocks are the
    unit of communication with the disk). This might be inefficient and
    thus must be weighed while designing an operating system

-   Size of data block pointers

    The larger the pointer, larger is the extent of the disk that can be
    accessed. However larger pointers take up more space in the form of
    file allocation tables and free-list management structures.

Other factors

-   
