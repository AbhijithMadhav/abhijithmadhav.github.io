---
title: Semaphores
---
Semaphore

-   A semaphore is an integer variable that apart from initialization,
    is accessed only through two standard atomic operations: P()(down())
    and V()(up()).
-   Semaphores don't become negative even after successive P() after it
    is zero

Typical usages

-   As a binary semaphore(mutex)(Initial value = 1)

    -   Used to implement mutual exclusion between processes on a
        critical-section(Figure 21)
    -   The value of the semaphore thus ranges only between 0 and 1
        signifying that “critical-section has not been occupied” and
        “Critical-section has been occupied” respectively

-   As a counting semaphore(initial value equal to the number of
    resources available)

    -   Used to implement access to a given resource/s between multiple
        contenders(Usually numbering more than the number of resources)
    -   The value of the semaphore ranges between 0 and +N, where the
        semaphore value indicates the number of available resources.

-   For scheduling constraints(Initial value = 0)

    -   When a thread must wait for a certain condition.
    -   Implementing thread.join()

Why is programming with semaphores is tricky?

As shown above semaphores can be used in different ways. In a typical
synchronization scenario there is a need for

-   implementing mutual exclusions and
-   taking care of scheduling constraints.

The order in which the above must be taken care off matters and is
subtle. It is semantically difficult to distinguish the different orders
though a particular order may result in a deadlock(Figure 28). Hence
there is a need of a higher order synchronization construct to mandate
this execution sequence. This is done in the implementation of a monitor
construct in a programming language(as in Java) or by specifying a
monitor programming paradigm(as in the pthreads API where it is
necessary to obtain a mutex lock before waiting or signaling on a
condition variable)

Spin-locks(Figure 20)

-   The processes busy-waits or spins in the CPU while waiting.

    -   Wastes CPU cycles
    -   Takes CPU cycles away from the process holding lock

-   Spin-locks may be suitable if a process.?????

-   Spinlocks are not appropriate for single-processor systems because
    the condition that would break a process out of the spinlock could
    be obtained only by executing a different process. If the process is
    not relinquishing the processor, other processes do not get the
    opportunity to set the program condition required for the first
    process to make progress. In a multiprocessor system, other
    processes execute on other processors and thereby modify the program
    state in order to release the first process from the spinlock.

Blocking semaphores(Figure 21)

-   Eliminates the busy waiting present in spin-locks. Processes block
    instead.

<span id="anchor"></span>Priority inversion

-   There are three processes L, M and H, whose priorities follow the
    order L \< M \< H.
-   M may run before H by preempting L if H is waiting on a semaphore
    for L.

Priority inheritance protocol

-   A protocol to prevent priority inversion
-   All processes accessing a shared resource needed by a
    higher-priority process inherit the higher priority until they are
    finished with the resource in question.

Implementing semaphores using testAndSet(Figure 30)
