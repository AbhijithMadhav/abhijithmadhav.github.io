---
title: Peterson's solution to the critical-section problem(Software solution)
---
Two – process solution

Note:(http://www.alper.net/programming/peterson%E2%80%99s-solution-on-modern-multiprocessors/)

Most modern CPUs reorder memory accesses to improve execution
efficiency. Peterson's solution will fail there as below.

Consider two threads running on separate processors and both wanting to
enter the critical section. Each thread sets its own interested array
slot (interested\[pid\] = TRUE) and then loads from the others slot
(interested\[other\] == TRUE). Both threads could read the old values
(FALSE, if stores are reordered after loads) and then assume that the
other party is not interested. It means both enter the critical section.

Such processors invariably give some way to force ordering in a stream
of memory accesses, typically through a [memory
barrier](http://en.wikipedia.org/wiki/Memory_barrier) instruction.
Implementation of Peterson's and related algorithms on processors which
reorder memory accesses generally requires use of such operations to
work correctly to keep sequential operations from happening in an
incorrect order.
