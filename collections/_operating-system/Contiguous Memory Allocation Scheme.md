---
title: Contiguous memory allocation scheme
---

-   Memory is divided into two partitions

    -   One for the resident operating system(low or high memory, linux
        occupies high
        memory)[<sup>\[1\]</sup>](http://en.wikipedia.org/wiki/High_memory)
    -   One for user processes

-   Each process is contained in a single contiguous section of memory,
    i.e., the physical address space of processes must be contiguous.

-   Memory mapping and protection is achieved through the use of a
    relocation and limit registers. These are populated by the OS as a
    part of the context switch for each process

-   The use of the relocation register also allows the size of the
    operating system to change dynamically

Memory allocation

-   Fixed partition scheme

    -   User memory is divided into **fixed** size partitions, each of
        which contain a user program.
    -   When a partition is free, a process is selected from the input
        queue and loaded into the free partition.
    -   When a process terminates, the partition becomes available for
        another process
    -   Was used by the IBM OS/MFT operating system. Is no longer in
        use.

-   Variable partition scheme

    -   OS keeps a table indicating which parts of memory are available
        and which are occupied

    -   A process is loaded into memory only if there is a hole big
        enough to accommodate it. Holes larger than the process are
        split to create ones just big enough for the process.

    -   When the process terminates, it releases its block of memory,
        which is then considered as another available hole. If this hole
        is adjacent to another one, it is merged with the other to
        create a larger hole.

    -   The **dynamic storage-allocation problem** of how to satisfy a
        request of size *n* from a list of free holes is solved by one
        of the below mentioned strategies

        -   **First fit** – Contributes to external fragmentation.
        -   **Best fit** – Produces the smallest leftover hole. Results
            in a lot of external fragmentation as this hole might not be
            usable
        -   **Worst fit** – Produces the biggest leftover hole which is
            more useful.

    -   External fragmentation

        -   Exists when there is enough memory space to satisfy a
            request of loading a process to memory but is not contiguous

        -   Fifty percent rule - Statistical analysis of first fit
            scheme reveals that given *N* allocated blocks, another *0.5
            N* blocks will be lost due to fragmentation.

        -   Solution

            -   Compaction

                -   Too costly
                -   Not possible if relocation is static and done at
                    assembly or load time. Possible only if relocation
                    is dynamic and is done at execution time

References

-   Operating system concepts – 8<sup>th</sup> edition – Silberschatz,
    et al.
