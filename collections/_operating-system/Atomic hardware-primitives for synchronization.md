---
title: Atomic hardware-primitives for synchronization
---
Why can't we have processes disable interrupts when entering
critical-section? Wouldn't this solve the critical-section problem?

On a multiprocessor system disabling interrupts on one processor would
not stop another processor from accessing memory and entering the
critical-section through a different processor at the same time.

# Hardware-atomic-primitives

Based on locking the shared memory bus. This will work for
multi-processor systems.

1.  testAndSet(&lock)

-   -   **Atomically** does the equivalent of the below

else

{

\*lock = TRUE;

return FALSE;

}

}

-   -   Solution to critical-section problem

<!-- -->

-   -   Declare that I am waiting to access the critical-section

    waiting\[i\] = true;

    -   Wait until either

        -   Some **older** process who has just now exited the
            critical-section says that I can enter the critical-section
            or
        -   The door is unlocked to the critical section by some other
            process who has just now exited the critical-section

    while (waiting\[i\] && TestAndSet(&lock));

    -   Declare that I am no longer waiting and finish my work in the
        critical-sections

    waiting\[i\] = false;

    // critical-section

    // ...

    -   Find the next process in the **order of their arrival**, who has
        declared an interest in entering the critical section

    j = (i + 1) % n;

    while((j != i) && !waiting\[j\])

-   -   If there are no such processes, unlock the door to the critical
        section

    if (j == i) lock = false;

    -   If there is such a process, ask it to enter the critical section
        thus handing the key over to it implicitly

    else waiting\[j\] = false;

    -   The key feature of the above algorithm is that a process blocks
        on the AND of the critical section being locked and that this
        process is in the waiting state. When exiting a critical
        section, the exiting process does not just unlock the critical
        section and let the other processes have a free-for-all trying
        to get in. Rather it first looks in an orderly progression (
        starting with the next process on the list ) for a process that
        has been waiting, and if it finds one, then it releases that
        particular process from its waiting state, without unlocking the
        critical section, thereby allowing a specific process into the
        critical section while continuing to block all the others. Only
        if there are no other processes currently waiting is the general
        lock removed, allowing the next process to come along access to
        the critical section.

-   -   swap(&lock, &key)

        -   **Atomically** does the equivalent of a swap

        -   Can use

            key = true;

            while(waiting\[i\] && key) swap(&lock, &key)

            in the above solution instead of

    while (waiting\[i\] && TestAndSet(&lock));

Do applications use the hardware-atomic-primitives directly?

No. It will be difficult programming for applications to directly use
them. Applications use semaphores(Offered by the OS) or monitors(Offered
as programming language constructs) to avoid race-conditions.
