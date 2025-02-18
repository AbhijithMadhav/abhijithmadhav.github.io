---
title: Process Synchronization
---
Race condition

A situation where several processes access and manipulate the same data
concurrently/simultaneously and the outcome of the execution depends on
the particular order in which the access takes place.

What is the root cause of race conditions?

-   In single processor architectures

    Preemption. Race-conditions are caused when processes are preempted
    while updating shared data-structures.

-   In multi-processor architectures

    -   Due to the inherent parallelism where two or more processes
        might be simultaneously updating shared data-structures
    -   Preemption of course

Can a pattern be seen in the data inconsistencies formed due to
race-conditions?

-   Process A accesses memory location m.
-   Scheduler allocates CPU to process B.
-   Now process B updates m.
-   Scheduler allocates CPU to process A.
-   Process A continues with the earlier obtained value of m.

Why is a non-preemptive kernel free of race-conditions on only kernel
data structures?

A non-preemptive kernel does not allow a process running in kernel mode
to be preempted. Processes running in user mode can be preempted and
race conditions can thus exist among user processes.

Example: Producer and consumer processes

Critical section(Figure 19)

That part of the the segment code of a process where shared resources
maybe modified.

Ex: Updating a table, writing a file, Changing common variables.

Critical-section problem?

Designing a protocol that processes can use to cooperate and thus avoid
race conditions.

Complete requirements of the solution of the critical-section problem

-   **Mutual exclusion** amongst processes in access of common
    critical-sections
-   **Bounded waiting** for every process wanting to enter
    critical-section.
-   **Progress** - Only contenders wanting to access the common
    critical-section(i.e., processes in their entry section) must form a
    factor in deciding who enters the critical-section and not every
    process which has this common-critical section(i.e., processes in
    their reminder section). Refer to
    [this](https://www.gatementor.com/viewtopic.php?f=267&t=10262)

How can race condition be avoided?(Figure 26)

By ensuring that only one process executes at a time in a **common**
critical region. This can be achieved by the use of

-   [Lo](Locks)[c](Locks)[ks](Locks)

<!-- -->

-   [Using Hardware-atomic-primitives
    directly](Atomic%20hardware-primitives%20for%20synchronization)
-   [Semaphores](Semaphores)
-   [Monitors](Monitors) - Typically implemented using semaphores
-   **An application specific software solution** using only atomic
    loads and stores(Think Too much milk solution)

Examples of kernel data structures that are prone to race conditions

-   Data-structure that maintains a list of all open files
-   Structures for maintaining memory allocation
-   Structures for maintaining process lists
-   Structures for interrupt handling

**Note:** The operations causing race-conditions are insertion and
removal from list
