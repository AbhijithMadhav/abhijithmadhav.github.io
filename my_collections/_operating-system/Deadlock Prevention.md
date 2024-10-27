---
title: Deadlock prevention
---
Ensure that atleast one of the conditions does not hold

-   Ensure that mutual-exclusion does not hold

    -   Cannot afford to prevent deadlocks by denying the
        mutual-exclusion condition because some resources are
        intrinsically non-shareable.
    -   Can deny mutual-exclusion on sharable resources like granting
        simultaneous access to read-only files.

-   Ensure that hold-and-wait does not hold.

    -   Get-All approach

        -   Require each process to request and be allocated all its
            resources before it begins its execution. Applications
            should thus be designed so that system calls requesting
            resources precede all other system calls.
        -   Disadvantage : Device utilization will be low.

    -   Get-One-At-A-Time approach

        -   Mandate that a process request resource only when it has
            none, i.e., it has to release its previously held resource
            before it can be granted a new one.
        -   Disadvantage : Starvation of a process that needs several
            popular resources is possible.

-   Ensure that preemption of granted resources is possible

    -   Surrender held resources if the current request can't be
        granted.
    -   Preempt and obtain resources of a waiting(waiting for other
        resources) process. If demand can't be meet by available
        resources from the free pool and those held by waiting
        processes, wait. This approach is used only for resources whose
        state can be easily saved and restored like CPU registers and
        memory.

-   Ensure that a circular wait is not possible

    -   One way to ensure that this condition never holds is to impose a
        total ordering of all resource types and to require that each
        process requests resources in an increasing order of enumeration
    -   Think of it this way. Suppose that there are 2 resources R1 and
        R2. A and B get in a circular wait only when they are capable of
        acquiring resources out of order, say, A gets hold of R1, B gets
        hold of R2 and now A and B wait on resources held by the other.
        If an ordering is mandated, say R1 and then only R2, then one of
        A and B, say A, will get hold of R1 while the other will wait on
        R1. A will the get R2, finish its job and release R1 and R2. B
        can now proceed.
