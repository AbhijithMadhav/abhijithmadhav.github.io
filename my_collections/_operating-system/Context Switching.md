---
title: Context-switching
---

-   Interrupt occurs
-   OS saves current context into PCB(save registers, process state and
    memory-management info of current process)
-   Perform many architecture-specific operations, including flushing
    data and instruction caches and TLB's.
-   Put current process into the ready queue
-   Choose the new process to execute
-   Load context from the PCB of that process
-   Start executing

<!-- -->

Multiple register sets

ULTRA-SPARC has it. Context switching is faster if the new context is
already in one of the register-sets compared to machines with no
multiple register-sets.

Context-switching is slower than single-register-set machines if new
context is in memory as one of the current register sets have to be
selected.
