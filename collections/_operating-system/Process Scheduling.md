---
title: Process scheduling
---
What is a long-term-scheduler ? How does it control the degree of
multiprogramming?

-   Existent only on earlier batch systems. More jobs/processes are
    submitted at once than can be loaded into memory immediately. The
    long-term-scheduler picks from this pool and loads them into memory
    for execution. It thus controls the degree of multiprogramming.
-   It needs to be invoked only when a process terminates and leaves the
    system. Because of the longer intervals(minutes) between executions
    it can afford to take more time to decide which process to pick
    unlike the CPU scheduler.
-   Failure by the long-term-scheduler to pick a good mix of CPU-bound
    and I/O-bound process will result in non-optimal usage of the CPU or
    I/O devices or both.
-   Time sharing systems like Unix and Windows have no
    long-term-schedulers and all new processes are put in memory for the
    short-term-scheduler.

Medium-term scheduler(Figure 5)

-   It is sometimes advantageous to remove processes from memory(and
    from active contention for the CPU) and thus reduce the degree of
    multiprogramming.
-   Later, the process can be reintroduced into memory, and its
    execution can be ontinued where it left off.
-   This swapping out and swapping in of processes is done by the medium
    term scheduler.
-   Swapping may be necessary to improve the process mix or because a
    change in memory requirements has over committed available memory,
    requiring memory to be freed up.

Short-term or CPU scheduler(Figure 5)

Selects from among the processes that are ready to eecuter and allocted
the CPU to one of them

Observed characteristic of processes w. r. t. scheduling(Figure 13 and
14)

-   Process execution consists of a cycle of CPU execution and I/O wait.
    Processes alternate between these states.
-   Also most of the CPU bursts are of shorter duration.

Non-preemptive scheduling

-   Is a scheduling scheme where scheduling takes place only when an
    executing process gives up the CPU voluntarily

    -   As a result of an I/O request or an invocation of wait for the
        termination of one of the child processes.
    -   When the process terminates

-   Cost

Preemptive scheduling

-   Scheduling may take place not only when a process voluntarily gives
    up CPU but also involuntarily when an interrupt is received, i.e.,
    when

    -   An interrupt has to be serviced
    -   Another process finishes I/O

-   Cost

    -   All system-calls of a **preemptive** kernel should be
        re-entrant.
    -   Access to shared data by processes must be synchronized. The
        access must also be deadlock free.

-   **Preemptive scheduling in a non-preemptive kernel** only preempts
    processes running in user-mode

Reentrant subroutine

-   A subroutine is called reentrant if it can be interrupted in the
    middle of its execution and then **safely** called again
    ("re-entered") before its previous invocations complete executing.

-   The interruption could be caused by an internal action such as a
    jump or call or by an external action such as a hardware interrupt
    or signal

-   Global data affect reentrancy of a subroutine and their use is not
    advisable.

    -   Global variables which are shared by all functions.
    -   Static variables which are shared by several instances of the
        same function.

## Re-entrant Kernels

Several processes may be executing in the kernel mode at the same time,
i.e. system-calls must be reentrant. No need for a system call to
complete before a process is preempted by the scheduler. If the kernel
is not re-entrant, a process can only be suspended while it is in user
mode (to be more precise, it could be suspended in kernel mode, but
would block kernel mode execution on all other processes).

Why can't interrupts be kept aside for later inspection?

They may be lost or overwritten

Scheduling criteria

-   CPU utilization
-   Throughput
-   Turnaround time
-   Waiting time
-   Response time

Why is it important for the scheduler to distinguish I/O-bound programs
from CPU-bound programs?

I/O-bound programs have the property of performing only a small amount
of computation before performing IO. CPU-bound programs, on the other
hand, use their entire quantum without performing any blocking IO
operations. Consequently, one could make better use of the

computer’s resources by giving higher priority to I/O-bound programs and
allow them to execute ahead of the CPU-bound programs.

[**Scheduling algorithms**](../Scheduling%20algorithms.odt)

|                                                                                           |            |                            |            |                 |               |                                        |
|-------------------------------------------------------------------------------------------|------------|----------------------------|------------|-----------------|---------------|----------------------------------------|
|                                                                                           | Preemption | CPU and device utilization | Throughput | Turnaround time | Response time | Waiting time                           |
| [FCFS](../Scheduling%20algorithms.odt#FCFS)                                               | No         | Low due to convoy effect   |            |                 | Bad           | High                                   |
| [SJFS/SNCBS](../Scheduling%20algorithms.odt#SJFS)                                         | No         |                            |            |                 |               | Minimum                                |
| SRTFS                                                                                     |            |                            |            |                 |               |                                        |
| [Priority scheduling](../Scheduling%20algorithms.odt#Priority)                            | Yes        |                            |            |                 |               |                                        |
| [Round Robin](../Scheduling%20algorithms.odt#Round-robin)                                 | Yes        |                            |            |                 | Good          | High when there are multiple processes |
| [Multilevel Q scheduling](../Scheduling%20algorithms.odt#Multilevel-Q)                    | Yes        |                            |            |                 |               |                                        |
| [Multilevel Feedback Q Scheduling](../Scheduling%20algorithms.odt#Multilevel-Q-Feedback') | Yes        | Good                       |            |                 |               |                                        |
| [Highest response ratio first](../Scheduling%20algorithms.odt#HRRF)                       |            |                            |            |                 |               |                                        |

Scheduling issues involving threads

-   Contention scope

    -   Process-contention scope

        -   Threads of the same process contend amongst themselves for
            scheduling
        -   Happens when the user-level thread-library schedule's
            threads in a many-to-one and many-to-many model

    -   System-contention scope

        -   Threads of all processes in a system contend amongst
            themselves for scheduling
        -   Happens when the kernel schedule's the user threads as in a
            one-to-one model.

-   On systems using either many-to-one or many-to-many, the two
    scheduling scopes are fundamentally different. On systems using
    one-to-one, PCS and SCS are the same.

Scheduling in Multiprocessors

-   Assumption

    -   All processors are homogeneous
    -   Scheduling can still not be done uniformly on them because there
        might still be differences in terms of only certain I/O devices
        connected to the private bus of a particular processor.

-   Scheduling in multiprocessors

    -   Asymmetric multiprocessors

        -   A master processor runs a scheduling algorithm and does the
            scheduling on all other processors.
        -   This is simple as access to the ready-Q(A kernel
            data-structure) need not be synchronized

    -   Symmetric multiprocessors(SMP)

        -   All processors run their own scheduling algorithm and pick
            up processes from a **common ready-Q.**
        -   Access to the ready-Q must be synchronized.
        -   All modern OS's support SMP

-   Processor Affinity

    -   Migrating processes from one processor to another will involve
        validation and revalidation of caches of the two processors
        which is costly

    -   Operating systems provide system calls through which processes
        can specify that they do not want to migrate

        -   Soft affinity – OS attempts to not migrate the process, but
            provides no guarantee
        -   Hard affinity – OS provides a guarantee of not migrating
            processes opting for hard affinity

-   Load balancing

    -   Load balancing attempts to keep the workload evenly distributed
        across all processors which each have their own private ready-Q.
        On systems sharing a ready-Q it is unnecessary

    -   Two approaches to load balancing

        -   Push migration

            -   A **specific task** periodically checks the load on each
                processor and – if it finds an imbalance – evenly
                distributes the load by pushing processes from
                overloaded to idle/less-busy processors.

        -   Pull migration

            -   A idle processor pulls a process from a busy one and
                executes it

    -   Two approaches need not be mutually exclusive. Linux runs the
        push task every 200 milliseconds and every individual processor
        runs the pull task when their ready-Q is empty.

**Suppose that a scheduling algorithm (at the level of short-term CPU
scheduling) favors those processes that have used the least processor
time in the recent past. Why will this algorithm favor I/O-bound
programs and yet not permanently starve CPU-bound programs? **

It will favor the I/O-bound programs because of the relatively short CPU
burst request by them; however, the CPU-bound programs will not starve
because the I/O-bound programs will relinquish the CPU relatively often
to do their I/O.

Discuss how the following pairs of scheduling criteria conflict in
certain settings.

-   **CPU utilization and response time **

    CPU utilization is increased if the overheads associated with
    context switching is minimized. The context switching overheads
    could be lowered by performing context switches infrequently. This
    could however result in increasing the response time for processes.

-   **Average turnaround time and maximum waiting time**

    Average turnaround time is minimized by executing the shortest tasks
    first. Such a scheduling policy could however starve long-running
    tasks and thereby increase their waiting time.

-   **I/O device utilization and CPU utilization **

    CPU utilization is maximized by running long-running CPU-bound tasks
    without performing context switches. I/O device utilization is
    maximized by scheduling I/O-bound jobs as soon as they become read

Complications that concurrency adds to an operating system.

-   Kernel should be reentrant
-   Limits should be imposed on resource usage and access by processes
-   Deadlock prevention
