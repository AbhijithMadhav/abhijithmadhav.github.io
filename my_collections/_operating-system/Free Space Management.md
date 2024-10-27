---
title: Free Space Management in Secondary Storage
---
Free-space list

A data structure which records all free disk blocks – those not
allocated to some file or directory. The free disk blocks listed here
are made use during allocation. The free-space list is augmented when
files are deleted

Bit Vector or Bit Map

-   Free-space list is implemented as a bit-map or a bit-vector with
    each block represented by 1 bit. If the block is free, the bit is 1;
    if the block is allocated the bit is 0.

-   Note: Even though bitmaps are data structures which make sense only
    during the running of an operating system, they must still be backed
    up on storage as a crash of an operating system necessitates the
    restoring of the operating system to its previous state which
    includes the previous state of memory allocation.

-   Advantages

    -   This implementation is relatively simple.

    -   Efficient operations in terms of finding the first free block or
        n consecutive free blocks.

        -   To find the first free block check successive words of the
            bitmap until a non 0 word is obtained. This word is then
            scanned linearly to find the location of the free block

-   Disadvantages

    -   Bit vectors are inefficient unless kept entirely in main memory.
        This may not be possible for large disk with big bitmaps.

Linked list

-   Maintain all the free blocks as a linked list, with the pointer to
    the first free block in the list cached.
-   Although traversing the list is inefficient, the most frequent
    operation is just accessing the first free block for the purposes of
    allocation.
-   If the pointer to the free list is lost, reconstruction of the free
    list would entail garbage collection.
-   A scheme to ensure that the pointer is never lost due to memory
    failure is to store it on the disk.

Grouping disk address in free blocks

-   A modification of the linked list approach.
-   The first of n free blocks stores addresses of the remaining m free
    blocks and so on
-   The addresses of large number of free blocks can now be found
    quickly unlike the linked list approach.

Storing count of contiguous free blocks in free blocks

-   Makes use of the fact that several contiguous blocks may be
    allocated or freed simultaneously, particularly when space is
    allocated through contiguous-allocation algorithm or clustering
-   Each entry of the free list now consists of a disk address and a
    number signifying the number of contiguous free blocks ahead.
-   Theses entries can be stored in a B-tree rather than a linked list
    for efficient lookup, insertion, and deletion.

Some file systems allow disk storage to be allocated at different levels
of granularity. How could we take advantage of this flexibility to
improve performance? What modifications would have to be made to the
free-space management scheme in order to support this feature?

Such a scheme would decrease internal fragmentation. If a file is 5KB,
then it could be allocated a 4KB block and two contiguous 512-byte
blocks. In addition to maintaining the usual structure to keep track of
free blocks, one would also have to maintain extra state regarding which
of the sub-blocks are currently being used inside a block. The allocator
would then have to examine this extra state to allocate sub-blocks and
coallesce the sub-blocks to obtain the larger block when all of the
sub-blocks become free.
