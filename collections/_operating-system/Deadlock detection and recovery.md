---
title: Deadlock detection and recovery
---
Deadlock detection

-   The system is already in a deadlock.

-   The system provides

    -   A deadlock-detection algorithm and
    -   A deadlock-recovery algorithm.

-   In this method the downside is not only the runtime overhead of
    maintaining necessary information and running of algorithms but also
    of the losses inherent in recovering from a deadlock.

<span id="anchor"></span>Deadlock-detection algorithms

-   Systems having only a single instance of each resource - Determining
    cycles in a **wait-for graph**(n<sup>2</sup>), n is number of
    processes.(Figure 36)
-   Systems having multiple instances of each resource - Bankers
    algorithms(n<sup>2</sup>m), m is the number of resource-types.

<span id="anchor-1"></span>Detection algorithm usage

-   In the extreme case invoke it every time a resource request cannot
    be satisfied and a process must wait. This request might be the one
    which completes a cycle. This is very expensive though the exact
    request that triggered the deadlock can be identified
-   Invoke the algorithm when CPU utilization falls below a certain
    level or at fixed intervals. The exact request that caused triggered
    the deadlock cannot be identified.

Deadlock recovery

-   Process termination
-   Preempt resources
