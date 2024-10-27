---
title: Kernel memory allocation
---
How is memory allocated for user processes in a virtual-memory-system?

In a virtual-memory-system, memory for user processes is allocated by
allocating a frame from the list of free frames maintained by the kernel

Why is memory for the kernel process not allocated in the same way?

-   Whereas the pattern of user process requests for the memory can
    never be known in advance, the kernel requests memory to allocate
    well known data structures. Thus there is an opportunity to avoid
    the internal fragmentation which results in user memory allocation.
-   Pages allocated to user-mode processes do not have to be contiguous.
    However, certain hardware devices interact directly with physical
    memory – without the benigit of a virtual memory interface – and
    consequently expect memory residing in contiguous locations.

Kernel memory allocation schemes

-   Buddy system – Was used in pre 2.2-linux.
-   Slab allocation

Buddy system allocation

-   Free memory reserved for allocation to the kernel is initially
    maintained as a fixed sized contiguous segment.

-   Memory is allocated from this segment using a power-of-2-allocator.

    -   That is, the segment is divided into two equal buddy segments,
        with the left buddy being allocated to the kernel if it is the
        smallest chunk greater than the request
    -   Subsequent requests

-   Advantages

    -   Coalescing - Adjacent buddies can be combined to form larger
        segments.

-   Disadvantage

    -   Internal fragmentation due to power-of-2 allocation

Slab allocation

-   Slab – One or more physically contiguous pages.

-   Cache – Contains one type of pre-allocated kernel data structures or
    objects. Different types of caches(i.e. Each containing a different
    types object) are themselves contained in slabs.

-   Initially all the objects in cache's are marked free. The allocation
    algorithm, on request for memory, uses one of these free objects to
    service the request

-   A slab may be in 3 possible states

    -   Full – All objects are marked used
    -   Empty – All objects are marked free
    -   Partial – Consists of free and used objects.

-   The allocator tries to satisfy requests with free objects, first
    from the paritally full slab, and then from an empty slab. A new
    slab is allocated as the last resort

-   Advantages

    -   Since the memory allocation is precise, no internal
        fragmentation
    -   Memory requests can be quickly satisfied as the objects are
        pre-allocated.

-   Solaris 2.4 onwards and linux 2.2 onwards use the slab allocation
    method for allocating memory to the kernel

The slab allocation algorithm uses a separate cache for each different
object type. Assuming there is one cache per object type, explain why
this doesn’t scale well with multiple CPUs. What could be done to
address this scalability issue?

This had long been a problem with the slab allocator – poor scalability
with multiple CPUs. The issue comes from having to lock the global cache
when it is being accesses. This has the effect of serializing cache
accesses on multiprocessor systems. Solaris has addressed this by
introducing a per-CPU cache, rather than a single global cache.
