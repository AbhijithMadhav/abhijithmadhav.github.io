---
title: Shared Memory(Figure 6)
---
-   A segment of the data space of a process is shared with another
    process for communication.
-   Processes t**hemselves are responsible** for synchronization. OS
    only sees to it that it does not enforce the “one process cannot
    write into the address space” of others.
-   SM is faster than message-passing as there is no copying of messages
    between processes and no kernel intervention is required to that as
    in message passing.

Typical shared-memory scenario

Producer consumer problem. Producer produces into an array. Consumer
consumes from it. Producer and Consumer must take care not to access the
array simultaneously and also not to produce or consume inappropriately.

Shared memory in unix

-   Implemented as a XSI IPC

-   Synchronization between the two processes sharing memory can be done
    using XSI semaphores or record-locking

-   int shmget(key_t key, size_t size, int flag);

    -   To create or access a shared-memory region.

-   int shmctl(int shmid, int cmf, struct shmid_ds \*buf);

    -   To tinker with the attributes of a shared-memory region.

-   void\* shmat(int shmid, const void \*addr, int flag);

    -   Attach to a already existing shared-memory region

-   int shmdt(void \*addr);

    -   Delete a shared-memory region
