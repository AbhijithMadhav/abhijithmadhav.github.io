---
title: Locks
---
-   A **lock** is a synchronization mechanism for enforcing limits on
    access to a resource in an environment where there are many threads
    of execution. A lock is designed to enforce a mutual exclusion
    concurrency control policy.
-   Locks however, provide means of **mutual exclusion only**. A typical
    synchronization problem also needs tools to implement resource
    control and scheduling constraints.
-   A resource is associated with a lock which must first be acquired by
    the process wishing access to the resource.
-   **lock.acquire() and unlock.release() primitives** are used by
    applications to guard their critical regions and achieve
    synchronization.

Implementations of lock primitives

-   **Atomic loads and stores(Also called **[**software
    solutio**](../Petersons%20Solution.odt)[**n or Petersons
    solution**](../Petersons%20Solution.odt)**)**

<!-- -->

-   -   Implementing a concurrent program based only on atomic stores
        and loads would be tricky and error prone.
    -   Hard to prove the correctness of a problem.
    -   Probably every solution would be problem specific and a common
        implementation of
        *l**o**c**k**.**a**c**q**u**i**r**e*()
        and
        *l**o**c**k**.**r**e**l**e**a**s**e*()
        would not be possible.

-   By disabling and enabling interrupts

    -   Naive implementation**(Figure 27 – 1)**

        -   If a rogue/buggy application never release an acquired lock
            then the system is no longer able to do I/O as acquiring a
            lock meant disabling interrupts.
        -   If critical-sections are arbitrarily long important I/O
            events are lost as interrupts are disabled.
        -   On a multiprocessor system disabling interrupts on one
            processor would not stop another processor from accessing
            memory and entering the critical-section through a different
            processor at the same time.
        -   Disable of interrupts for all processors in a multiprocessor
            system is very costly.

    -   A better solution would be(**(Figure 27 – 2)**

        -   
            *l**o**c**k**.**a**c**q**u**i**r**e*()
            and
            *l**o**c**k**.**r**e**l**e**a**s**e*()
            atomically test and set a lock variable. Interrupts are
            disabled/enabled only for the purposes of testing and
            setting this variable.

        -   Now, interrupts are disabled and enabled in both the
            constructs, acquire() and release(), and thus there is no
            chance for a rogue/buggy app to permanently disable
            interrupts

        -   Also interrupts are disabled for a short duration of testing
            and possibly setting the lock variable. Thus the length of
            the critical section of an app will not result in IO events
            getting lost.

        -   Disadvantages

            -   Sleep() must enable interrupts as its first step or

            -   A woken up process/thread as its first step must enable
                interrupts(Nachos kernel code).

                -   Modifications to code and assumptions of program
                    state cannot be localized to a small piece of code
                -   Writing such programs is tricky and there is a
                    chance that it acquires bugs as people modify code.

-   Atomic hardware primitives like testAndSet(), swap()(Figure 28 – 1,
    2)

    -   Naive implementation**(Figure 28 – 1)**

        -   Busy-waiting wastes CPU cycles

    -   A better solution**(Figure 28 – 2)** would be for and to
        busy-wait only to test and set a mutex variable which regulates
        access to the lock variable. Interrupts are disabled/enabled
        only for the purposes of testing and setting this mutex.

    -   Disadvantages

        -   [Priority inversion](../Semaphores#Priority%20inversion)
