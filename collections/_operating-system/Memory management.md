---
title: Memory management
---
Memory management schemes

-   [Contiguous memory allocation
    schemes](Contiguous%20Memory%20Allocation%20Scheme)
-   [Paging](../../Computer%20Organization%20and%20Architecture/Virtual%20Memory.odt)
-   [Segmentation](Segmentation)

Advantages of paging over contiguous memory allocations schemes

-   Permits the physical address space of process to be non-contiguous
    and thus avoids external fragmentation and the need for compaction
-   Avoids external fragmentation in the secondary memory. This happens
    as memory chunks of varying size do not have to be swapped in and
    out of secondary storage.

Conceptual difference between paging and segmentation

The main difference is in the programmers view of memory. In paging, the
memory layout of the process is still contiguous as the operating system
handles the inner intricacies. In segmentation the memory layout is
visible as one composed of segments of the program.

Segmentation with paging

-   The address space is segmented as in segmentation

-   Each individual segment is paged.

-   Segmentation and paging are often combined in order to improve upon
    each other.

    -   **Segmented-paging** is helpful when the page table becomes very
        large. A large contiguous section of the page table that is
        unused can be collapsed into a single segment table entry with a
        page table address of zero.
    -   **Paged-segmentation** handles the case of having very long
        segments that require a lot of time for allocation. By paging
        the segments, we reduce wasted memory due to external
        fragmentation as well as simplify the allocation.

Why is sharing easier in a segmentation model than in a paged model?

Sharing usually occurs for a logical quantity, like the code of a
program, a data structure like an array or a symbol table. Segmentation
which is based on the logical division of memory is more amenable as
such sharing involves only one segment table entry of the processes
wishing to share.

Paging is based on the physical division of memory. Logical resources
like the above may span more than one page and sharing involves multiple
page table entries in the sharing processes having the same physical
page numbers

Why is memory allocation in a paged model easier than in a segmentation
model?

In a paged model, the OS has to keep track of free frames of the
physical memory for the purposes of allocation. When allocation needs to
be done, it has to find a free frame(using a LRU scheme), or choose to
replace one if one doesn't exist.

In a segmentation model, the OS has to keep track of holes, like in a
contiguous allocation scheme, which is more complicated than keeping
track of fixed length frames. When allocation needs to be done, it has
to find a suitable hole. Due to external fragmentation it may not be
able to find one even though the total amount of free physical memory is
greater than the needs of the yet to be allocated segment. Compaction
may then be needed.

Comparison between the different Memory management schemes

<table>
<tbody>
<tr class="odd">
<td></td>
<td>Contiguous memory allocation</td>
<td>Paging</td>
<td>Segmentation</td>
</tr>
<tr class="even">
<td>External fragmentation</td>
<td>Yes</td>
<td>No</td>
<td>Yes</td>
</tr>
<tr class="odd">
<td>Internal fragmentation</td>
<td>No</td>
<td>Yes</td>
<td>No</td>
</tr>
<tr class="even">
<td>Code sharing</td>
<td>No</td>
<td>Yes</td>
<td>Yes</td>
</tr>
<tr class="odd">
<td>Dynamic memory allocation</td>
<td>Relocation of the entire address space of the program may be required if a contiguous free hole is not available.</td>
<td>Possible without any relocation. Just allocate a new page for the purpose.</td>
<td>Relocation of the concerned segment may be required if a contiguous free hole is not available.</td>
</tr>
<tr class="even">
<td>How is growth from both sides of the address space, i.e., from the stack and from the heap handled</td>
<td><p>Requires the operating system to allocate the entire extent of the virtual address space to the program when it starts executing. This could be much higher than the actual </p>
<p>memory requirements of the process. </p></td>
<td><p>Does not require the operating system to allocate the maximum extent of the virtual address space to a process at startup time, but it still requires </p>
<p>the operating system to allocate a large page table spanning all of the program’s virtual address space. When a program needs to extend the stack or the heap, it needs to allocate a new page but the corresponding </p>
<p>page table entry is preallocated.</p></td>
<td>Gives the operating system flexibility to assign a small extent to each segment at program startup time and extend the segment if required.</td>
</tr>
<tr class="odd">
<td>User view of logical address space</td>
<td>Contiguous</td>
<td>Contiguous</td>
<td>Segmented by design</td>
</tr>
<tr class="even">
<td>Actual physical address space</td>
<td>Contiguous</td>
<td>Non-contiguous</td>
<td>Non-contiguous</td>
</tr>
<tr class="odd">
<td>Amount of memory required to by address translation structures</td>
<td>– </td>
<td>An entry for every page in the logical address space.</td>
<td>Just two registers(base, limit) per segment. The number of segments is usually small.</td>
</tr>
</tbody>
</table>
