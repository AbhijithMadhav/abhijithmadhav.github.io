---
title: Threads
---
Motivation to have threads

-   So that one I/O bound execution path does not block the other
    execution paths of a process.
-   Managing and switching between threads is faster compared to that of
    processes.(Pseudo reason)
-   Provide with the ability to make use of the parallelism offered by
    multiprocessors.
-   Yield substantial performance gain for processes having a mix of
    CPU-bound and I/O-bound execution paths.
-   Ensure responsiveness in UI programs

Things that threads share

-   Code
-   Heap area
-   Resources like open files, memory, sockets, device handles, windows,
    child processes, pending alarms, signals, accounting information.
-   Global data

Things that threads don't share

-   Stack
-   Registers
-   PC
-   State

Why is creating a process slower than creating a thread?

Because a thread is smaller than a process, thread creation typically
uses fewer resources than process creation. Creating a process requires
allocating a process control block (PCB), a rather large data structure.
The PCB includes a memory map, list of open files, and environment
variables. Allocating and managing the memory map is typically the most
time-consuming activity. Creating either a user or kernel thread
involves allocating a small data structure to hold a register set,
stack, and priority.

What is meant by “Threads share memory among themselves”?

The address space of a process is all the memory a process is going to
use. This memory can be divided into code, global data, stack space and
heap. Code, global data and heap are shared by threads while stack
isn't.

However the individual stacks of the different threads can be accessed
by one another. There is no memory protection. Thus in totality one can
say that memory as a resource is shared between threads

Why is context-switching faster among threads of the same process than
context switching of processes?

-   In both cases register-set has to be reloaded.
-   The speed factor is mainly due to the fact that process have to save
    PCB related stuff like open files handles, device handles, sockets,
    windows and that a PCB is generally larger than a related thread
    management structure.

Threaded execution has a higher start-up speed than processes

During context-switch of processes, architecture-specific cache's and
TLB's will be invalidated or will atleast be of minimal use for a
different new process, This results in high percentage of cache-misses
and page-faults initially.

Difference between user-level and kernel-level threads(Figure 12)

The 'level' refers to where thread scheduling and management is done. If
the threads are user-level, the kernel has no knowledge about them.

User level threads(N:1)

-   Created and managed by a user-level thread-library. A runtime system
    of the library does this.
-   Applications which use user-level thread-library are portable.
-   True concurrent processing is not possible with user-level threads.
    If one thread of a process gets blocked, none of the others can
    execute too as the kernel does not know about them and simply
    schedules in a different process.
-   Creation and context-switch of threads is faster compared to kernel
    threads as everything is done in user mode. As a result apps which
    use user threads are generally faster on a **uniprocessor** machine.
-   user-level threads cannot be preempted by a timer interrupt in
    favour of another thread(of the same process) because the runtime
    system cannot receive any timer interrupt(It is also a part of the
    same process). The thread has to voluntarily enter the run-time
    system so that the scheduler has a chance of picking another thread.
-   Example: Gnu portable threads

Why is true concurrent processing not obtained by usage of user-level
threads in a multiprocessor machine?

The kernel is not aware of the multiple threads of execution contained
in a process. From its point of view the process is the basic
schedulable entity. Once it schedules a process(and hence one of it's
threads) on a processor it will not want to schedule the same
process(albeit a different thread) on a different processor.

Why are user-level threads faster to create and manage?

Because they don't have to trap to the kernel.

Kernel-level threads(1:1)

-   Created and managed by the kernel. The threads themselves are
    threads which belong to a user level process.
-   Apps which use kernel threads may not be portable if the OS does not
    implement a standard kernel thread interface like POSIX.
-   True concurrent processing can be obtained in multiprocessors.
-   Creation and context-switch of kernel threads is slower as a a trap
    has to be made to the kernel mode. This makes it considerably slower
    than user threads on a uniprocessor machine.
-   Thread implementation in Linux, UNIX and windows 95 to XP, Java
    threads

Hybrid threads(M:N)

-   A combination of kernel threads and user threads are used, i.e. The
    kernel has knowledge about M groups of threads of a process and not
    about the N threads in each individual group.
-   Threads inside a bunch are scheduled by the user library. The kernel
    schedules the bunches as a whole.
-   Context-switch among threads in the same bunch can happen fast as
    scheduling takes place at a user level.
-   True concurrency can be achieved on multiprocessor as the kernel
    will schedule a different thread of a group of the same process on
    different processor.
-   Threads from a different group are scheduled if a thread in the
    current group blocks
-   Thread implementation in windows 7

How can a runtime-system schedule threads in the case of user-level
threads when it is itself a user-level process scheduled periodically by
the kernel?(Figure - 12)

The runtime system is not a independent user level process. It is a
library whose code is bound to the code of the process compile-time or
run-time. As a result it is accurate to view the runtime system as a
part of the process and a per process entity.

Scheduler Activations

-   Attempt to exploit the functionality of kernel-level threads and the
    flexibility and ease of user-level threads.
-   Implementation is of a user-level thread-library with provisions for
    an **upcall-handler**. Changes are made to the kernel to allow for
    upcalls.
-   When a thread of a process blocks(on an I/O, page fault), the kernel
    makes an **upcall** to the runtime scheduler informing it about the
    thread that has blocked instead of itself scheduling a new process.
    The runtime-system schedules a different thread of the process. Thus
    a process is able to make use of its entire time quanta instead of
    being preempted by a blocking thread.
-   Also when the blocking thread is done with blocking the kernel makes
    another **upcall** informing the runtime-system of the user-level
    thread-library of the now ready thread so that it can run it now or
    schedule it later.
-   Objections to this type of implementation as it violates the
    cardinal rule of layered design, level n+1 makes use of services
    provided by level n. Upcalls result in layer n calling procedures in
    layer n+1.

Threading issues

-   Duplication of threads during a fork()

    -   Only the executing thread should be duplicated on hitting a fork
        if an exec will follow immediately. Else time spent in
        duplicating all threads will be wasted
    -   All threads should be duplicated if an exec will not follow.

-   Thread cancellation

    -   Asynchronous cancellation

        -   A thread cancels the target thread immediately. This is an
            issue if the thread is in the midst of updating data shared
            with other threads.

    -   Deferred cancellation

        -   The target thread checks periodically to see if it should
            cancel itself(cancellation points). It can then do so in an
            orderly fashion

-   Signal handling

Thread pools

-   Servicing a request with an already created thread is faster than
    creating a new thread.
-   A thread pool limits the number of threads that exist at any one
    point. This is particularly important on systems that cannot support
    a large number of concurrent threads.

Thread-Specific data

-   Think thread local variables. Need to read pthreads

Application suitable for threading

-   Interactive programs like debugger and editors which need a thread
    to monitor user input and another one to run the program etc
-   Parallelized applications like matrix multiplication and summing
-   Web server which services each request in a separate thread.
    Fetching a web-page for one request should not delay another
    concurrent request.

Application not suitable for threading

-   Applications which have data dependency like calculating an
    individual tax return
-   Application which are not CPU bound. Example?

Light Weight Process(LWP)

-   A LWP is a kernel data structure which is representative of a basic
    schedulable entity.
-   In a one-to-one model this means that the kernel has one LWP per the
    number of user threads per process.
-   In a many-to-one model the kernel has just one LWP per process.
-   In a many-to-many model the kernel has one LWP per a bunch of
    threads per process.
