---
title: Processes(Figure 1 and 2)
---
When will a process lose the CPU?

In preemptive scheduling,

-   When timer interrupt occurs, indicating that it's time slice is over
    in case of round robin scheduling.
-   Process issues a I/O request and then has to wait for the completion
    of the I/O.
-   Create and then wait for child's termination.
-   Other hardware interrupts.
