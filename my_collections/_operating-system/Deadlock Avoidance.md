---
title: Deadlock avoidance
---

## Safe state(Figure 33)

-   The current state in a system is said to be safe if the **operating
    system** can satisfy all future resource-requests of each process(up
    to its maximum) in some order and still avoid deadlock.
-   A safe state is not a deadlocked state.
-   A deadlocked state is an unsafe state.
-   Not all unsafe state lead to a deadlock. An unsafe state has a
    possibility of ending in a deadlock. The behavior of the processes
    determine whether a deadlock occurs or not. There is no ordering of
    processes(where each process requests all of its **remaining
    requirements at once**) the OS can employ such a deadlock is
    avoided.

## Safe sequence

A sequence of processes is a safe sequence for the current allocation
state if , for each P<sub>i</sub>, the resource requests that
P<sub>i</sub> can still make can be satisfied by the currently available
resources plus the resources held by all other P<sub>j</sub>, j \<
i(i.e. By processes finishing earlier than P<sub>i</sub> )

## General feature of avoidance algorithms

-   Whenever a process requests a resource that is currently available,
    the system must decide whether the resource can be allocated
    immediately of whether the process must wait. The request is granted
    only if the allocation leaves the system in a safe state.
-   As a process requesting a currently available resource may still
    have to wait., the resource utilization may be lower than what would
    have been otherwise.

## Deadlock-avoidance Algorithms

-   For systems having only a single instance of each resource -
    Determining cycles in a **resource-allocation
    graph**(n<sup>2</sup>), n is number of processes.
-   For systems having multiple instances of each resource - Bankers
    algorithms(n<sup>2</sup>m), m is number of instance types

### Bankers algorithm for deadlock-avoidance

-   When a new process enters the system, it must declare the maximum
    number of instances of each resource it needs.(should not exceed the
    resources that the system possesses).

-   When a process requests a resource or a set of resources

    -   The resource-request algorithm is invoked to simulate the
        allocation of resources
    -   The safety algorithms is invoked to determine whether the above
        allocation of resources results a safe resource-allocation
        state.

-   If a safe state is possible, the resources are allocated; Else the
    process must wait.

### Resource-request algorithm

-   When a request for resource is made by a process the following
    actions are taken

    -   If its request exceeds the availability the process must wait.
    -   Else simulate granting of resources.

### Safety algorithm

-   Find a process whose needs can be satisfied with the available
    resources in one go.
-   Mark the process as finished by simulating allocation of resources
    to it.
-   If all process can be allocated(simulation) the needed resources by
    repeating the above two steps, the system is in a safe state.
