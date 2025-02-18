---
title: Deadlocks
tags:
  - os
  - deadlocks
---
**Example of resource types managed by the operating system**

-   Memory space
-   CPU cycles
-   Files
-   IO devices(Printers and DVD drives)

**Sequence of operations a process follows while wanting to utilize a
resource(Figure 31)**

-   Request
    -   request() device
    -   open() a file
    -   allocate() memory
-   Use()
-   Release
    -   release() device
    -   close() a file
    -   free() memory

**How are resources not managed by the operating system managed?**

Through synchronization constructs

### Classification of resources

-   Physical resources

    Ex – Printers, tape drives, memory space and CPU cycles

-   Logical resources

    Ex – Files, semaphores, locks, monitors

**When are a set of processes said to be deadlocked?**

When all the processes in the set are waiting for an event that can be
caused only by another process in the set. 'Events' in this context mean
acquisition and release of resources.

### Necessary conditions for a deadlock to take place

-   Mutual exclusive access of resources
-   Hold and wait protocol of processes
-   No preemption of held resources policy by the operating system
-   Circular wait by processes for resources

### Representation of data structure used by operating systems to manage resources
```java
struct manage {

    resource id;

    resource_type type;

    bool allocated;

    pid processAllocatedTo;

    Node* HeadOfWaitQForThisResource;

} manageTable[numOfResources];
```


**Does a cycle in a system resource allocation graph imply a deadlock?**

-   It does so only if there are single instances of each resource
    type.(Figure32 - 2)
-   In case there are multiple resource types then the system may or may
    not be in a deadlock.(Figure 32 - 3)

**Does the absence of a cycle imply a deadlock free system?**

Yes

Methods for handling deadlocks

-   [Deadlock prevention](Deadlock%20Prevention)

    Prevent deadlocks by restraining how requests are made. These
    restraints ensure that at least one of the necessary conditions for
    deadlock does not hold.

-   [Deadlock avoidance](Deadlock%20Avoidance)

    Avoid deadlock by having the operating system moderate requests.
    Operating system is given advance additional information concerning
    the resources a process will request and use in its lifetime. With
    this additional knowledge, it can decide whether a request can be
    granted or should wait when a process makes one.

-   [Deadlock ](Deadlock%20detection%20and%20recovery)[detection
    and
    ](Deadlock%20detection%20and%20recovery)[recovery](Deadlock%20detection%20and%20recovery)

    System uses an algorithm to detect a deadlock and then recovers from
    it.

-   Not handling them at all.

Most popular way of handling deadlocks by operating system? Why?

Not handling them at all. In many systems, deadlocks occur very
infrequently(say, once per year); thus, this method is cheaper than the
prevention, avoidance, or detection and recovery methods, which must be
used constantly.

Usage of deadlock prevention/avoidance and deadlock detection and
recovery algorithms

Deadlock prevention/avoidance is commonly used if the probability of the
system entering the deadlock is relatively high, otherwise, detection
and recovery are more efficient.

Difference between deadlock prevention and deadlock-avoidance.

-   The onus of deadlock prevention is on applications. The applications
    have to be coded such that one of the necessary conditions do not
    hold. The OS can just aid the applications in terms of providing the
    data required. This data can be an allocation table, a mapping for
    ordering resource types etc.
-   The Operating system takes care of the implementation of
    deadlock-avoidance by monitoring the requests made by applications.
    This is like a policeman regulating traffic vs having only traffic
    lights.

Suppose processes, share identical resource units, which can be reserved
and released one at a time. The maximum resource requirement of process
is , where . What is the sufficient condition for ensuring that deadlock
does not occur?

1.   is not in deadlock with any other process if it has resources.
    Suppose that each has resources and is waiting for one more
    resource. All the processes are in deadlock if there is no other
    instance of the resource left.

-   Now, if we add just one more resource to the resource pool, all the
    processes will complete one after another, i.e., some resource will
    get that just added resource, finish its job, release all its
    resources so that other processes can finish theirs eventually.
-   Thus
