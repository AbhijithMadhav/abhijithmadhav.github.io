---
title: File Allocation methods
---
-   The aim is to allocate the space in the disk to files so that disk
    space is utilized effectively and files can be accessed quickly.
-   Some systems support multiple allocation methods. More commonly a
    system uses one method for all files within a filesystem type.

Contiguous Allocation

-   Requires that each file occupy a set of contiguous blocks on the
    disk.

-   Directory Entry - The directory entry of such a contiguously
    allocated file need contain only a pointer to the first block and
    the number of blocks that the file spans.

-   Data block access time – Supports both sequential and direct access
    efficiently

    -   Since disk addresses are linearly ordered, accessing block *b +
        1 *after *b* normally requires no head movement. A head movement
        to the next track is required only if *b + 1'th *block lies in
        the next track. Thus seek time is minimized to a large extent.
    -   Sequential access – The file system remembers the disk address
        of the last block referenced and, when necessary, reads the next
        block.
    -   Direct access – If byte *i *of a file that starts at block* b
        *is required, it can be immediately accessed as block *b + i.*

-   Storage efficiency - Not very good due to the following two
    problems.

    -   Allocating space for a new file

        -   This is the same as trying to satisfy a request a hole of
            size n from a list of free holes. Simulations show that
            first fit and best fit are more efficient than worst fit.
            None of the strategies are good in terms of storage
            utilization though. They all result in external
            fragmentation.

        -   Compaction

            -   Copy the entire file-system to another disk and or tape
            -   Reallocate on original disk freshly
            -   This scheme effectively compacts all free space into one
                contiguous space, solving the fragmentation problem.
            -   Cost of compaction is time and it can be particularly
                severe for large hard disks, where compacting all the
                space may take hours and may be necessary on a weekly
                basis.
            -   Offline compaction can be done with the file system
                unmounted. However this would require a downtime.

    -   Determining how much space is needed for a file at the time of
        creation of a file.

        -   In general, it is difficult for the system or the user to
            determine the size of the file in advance.
        -   If allocated space is too less for the file, a larger hole
            must be found, copy the contents of the file and release the
            previous space. This is inefficient.
        -   If allocated space is too much to prevent the above problem
            there might be a considerable wastage of space. Even if the
            space needed for a file is known in advance, preallocation
            may be inefficient if a file grows slowly over a long
            period.

Contiguous allocation with extents

-   A modified version the contiguous allocation scheme may be used.

    -   Files are created initially by allocating contiguous blocks.

    -   If the file grows bigger than the allocated block, another
        possibly non-contiguous block known as the **extent **is
        allocated with a pointer to it possibly in the directory entry
        of the file.

    -   A key issue in such systems is the degree of variability in the
        size of the extents

        -   All extents are of the same predetermined size

            -   Simplifies the block allocation. A simple bitmap or a
                free list of extents would suffice.
            -   Internal fragmentation might result due to
                **inflexibility** in allocation of the desired sized
                extent

<!-- -->

-   -   -   Extents can be of any size and are allocated dynamically

            -   Block allocation is complicated. A Buddy allocation
                system is needed.
            -   External fragmentation might also occur due to
                allocation and deallocation of various sized extents.

        -   Extents can be of a few fixed predetermined sizes

            -   A separate bitmap or free list has to be maintained for
                each size. Is of intermediate flexibility and complexity
                compared to the above two cases

        -   In terms of overhead in locating a random block this
            approach requires more overhead compared to the contiguous
            allocation scheme and less compared to the linked list
            scheme.

Linked list allocation

-   Data blocks of a file are allocated as nodes of a linked list with
    pointers to the next node as needed.

-   There is no external fragmentation due allocation

-   The size of the files need not be known in advance

-   Data block access time

    -   Direct access requires traversal of the linked list as data
        blocks are scattered throughout the disk and this results in
        possibly multiple head movements and thus large seek times.
    -   Sequential access also possibly requires multiple head
        movements.

-   Storage efficiency

    -   The pointers stored in each block to the next, reduce the amount
        of space available for file data and increases the space needed
        for block allocation and free list management.

        -   <span id="anchor"></span>The usual solution is to increase
            the amount of data stored per pointers by using clusters(a
            set of contiguous blocks). Effects of internal fragmentation
            may be severe though

-   Reliability – One pointer stored wrong or corrupted may have serious
    effects

FAT(File allocation table)

-   A variation of the linked allocation scheme. Used by MSDOS and the
    OS/2 operating system. Instead of maintaining the blocks of a file
    as a list, addresses of blocks are maintained as a list.
-   Each volume consists of an allocation table containing an entry for
    each block on the volume with its respective disk address. The
    entries corresponding to the blocks of a file are linked together.
-   A directory entry is an index to the entry in the table which
    consists of the address of the first block. Successive blocks can be
    found by following the pointer trail in the FAT. This is especially
    efficient in case of random access vis-a-vis the normal linked
    allocation scheme as typically, **most of the FAT can be cached in
    memory** and therefore the pointers can be determined with just
    memory accesses instead of having to access the disk blocks.

Indexed allocation

-   Indexed allocation solves the problem of linked list allocation,
    inefficient direct access, by bringing all the pointers together
    into one location, the **index block**.

-   The index block is a per file structure and is an array of
    disk-block addresses. The i'th entry in the index block points to
    the i'th block of the file.

-   Data block access times

    -   Since index blocks of files may be large, they may not always be
        available in memory. The directory entry of a file may just
        contain a pointer to the files index block. If the index block
        is in memory, the access can be made directly. If it isn't
        depending on the structure and size of the index block, possible
        multiple readings of the disk might be required.

-   Storage efficiency

    -   Suffers from wasted space. The pointer overhead of the index
        block is generally greater then the pointer overhead of linked
        allocation.

-   Structure of the index block

    -   Linked scheme – A list of index blocks
    -   Multilevel scheme – Indexes a block pointer which point to
        secondary index blocks which in turn point to actual data blocks
    -   Combined scheme – A proportion of the indexes in a index block
        point to data blocks while the rest point to secondary index
        blocks.

Practical usage

-   Some systems support contiguous allocation for files requiring
    direct access and linked allocation for files requiring only
    sequential access.
-   Some systems automatically start of with contiguous allocation and
    switch to indexed allocation if the files grow bigger.
-   Given the wide disparity in disk speed and CPU speed hundreds of
    lines of codes are added to the OS to save a few head movements and
    the like.

Consider a file currently consisting of *n* blocks. Assume that the
file-control block (and the index block, in the case of indexed
allocation) is already in memory. Calculate how many disk I/O operations
are required for contiguous, linked, and indexed (single-level)
allocation strategies, if, for one block, the following conditions hold.
In the contiguous-allocation case, assume that there is no room to grow
at the beginning but there is room to grow at the end. Also assume that
the block information to be added is stored in memory.

|                                          |                       |                   |                    |
|------------------------------------------|-----------------------|-------------------|--------------------|
| Operation                                | Contiguous Allocation | Linked Allocation | Indexed Allocation |
| The block is added at the beginning.     | 2n + 1                | 1                 | 1                  |
| The block is added in the middle.        | n + 1                 | 1 + n/2 + 1       | 1                  |
| The block is added at the end.           | 1                     | 1 + n             | 1                  |
| The block is removed from the beginning. | 2(n -1)               | 0                 | 0                  |
| The block is removed from the middle.    | n                     | n/2 + 1 + 1       | 0                  |
| The block is removed from the end.       | 0                     | (n-1)             | 0                  |

Logical to physical address mapping in different allocation schemes –
Not complete

Logical starting address of file = Z

Block size = *b* bytes

X =

Y = Z % b

|                   |                                                     |                                                                                                         |                                                                                                                               |
|-------------------|-----------------------------------------------------|---------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------|
|                   | Contiguous allocation                               | Linked allocation                                                                                       | Indexed allocation                                                                                                            |
| Physical address  | Access the Z + X'th block, Y'th byte in that block. | Chase down the linked list X times to obtain the required block and access the Y'th byte in that block. | Physical block address is contained in the X'th entry of the index table. Y gives the displacement of the byte in that block. |
| Number of disk IO | 1                                                   | X                                                                                                       | 1+1 (Assuming that index block has to be read into memory)                                                                    |

Consider a system that supports the strategies of contiguous, linked,
and indexed allocation. What criteria should be used in deciding which
strategy is best utilized for a particular file?

-   Contiguous—if file is relatively small.

-   Linked —if file is large and usually accessed sequentially.

-   Indexed —if file is large and usually accessed randomly.

Why is it advantageous for the user of an operating system to
dynamically allocate its internal tables? What are the penalties to the
operating system for doing so?

Dynamic tables allow more flexibility in system use growth — tables are
never exceeded, avoiding artificial use limits. Unfortunately, kernel
structures and code are more complicated, so there is more potential for
bugs. The use of one resource can take away more system resources (by
growing to accommodate the requests) than with static tables.
