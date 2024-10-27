---
title: Scheduling algorithms
---
<span id="anchor"></span>FCFS(First come first serve scheduling)

-   Process relinquishes CPU when its **current CPU-burst is over.**

-   Convoy effect

    -   Caused by one CPU-bound process and many I/O-bound process being
        present in an FCFS scheduling environment.
    -   The I/O-bound processes convoy behind the CPU-bound process for
        the CPU most of the time leading to low device I/O utilization.
    -   When the CPU-bound process is doing I/O, the I/O-bound processes
        quickly finish their CPU-burst and leave the CPU idle.

<span id="anchor-1"></span>SJFS(Shortest job first scheduling)

-   The next CPU-burst is the one taken into account. It should hence be
    called **Shortest-next-CPU-burst-scheduling(SNCBS)**

-   There is a difficulty in knowing the length of the next CPU burst.

    -   Typically not used in short-term scheduling.
    -   One option of using it with short-term scheduling is to predict
        the next CPU burst using an exponential average model.(Figure
        17)

-   Used in long term scheduling where user submits the process time
    limit which can be used to predict the burst times.

-   SRTFS(Shortest remaining time first scheduling)

    -   SJFS with preemption.

<span id="anchor-2"></span>Priority scheduling

-   Types of priorities

    -   Internal

        -   Use some measurable quantities to compute the priority of a
            process
        -   Examples – Time limits, memory requirements, the number of
            open files

    -   External

        -   Set by criteria outside the OS
        -   Example – importance of the process, the type and amount of
            funds being paid for computer use, the department sponsoring
            the work, business considerations, political considerations

-   Major problem here is of indefinite blocking or **starvation**

    -   A low priority process never executes due to continuous incoming
        high priority process
    -   Solution to this is implementing aging of low priority process.
        **Aging** is the technique of gradually increasing the priority
        of a process which waits for a long time.

<span id="anchor-3"></span>Round-Robin Scheduling

-   Process gives up CPU because it needs I/O or is
    interrupted/preempted by a timer interrupt indicating exhaustion of
    time quantum

-   Effect of length of time quantum

    -   Large time quantum – Basically a FCFS
    -   Small time quantum – Many context switches for the same process.
        Ensure that time quantum is larger relative to the
        context-switch time. Typical time quanta is 10-100 ms and
        context-switch time is 10 micro second making the context-switch
        time hog 0.1% – 1% of total execution time.

<span id="anchor-4"></span>Multilevel Q scheduling(Figure 15)

-   Suitable for situations in which process can be classified easily on
    the basis of different scheduling needs

    -   Examples of such classifications and their different scheduling
        needs

        -   Foreground and background processes – Different response
            time requirements

-   Here ready Q is subdivided into several separate Q's. Each contains
    processes with the same scheduling requirements. **Processes cannot
    move in-between Q's**

-   Each Q has its own scheduling algorithm.

    -   Example : Foreground process Q may run a RR algorithm and
        background process Q may run FCFS

-   All the above Q's themselves are scheduled using

    -   A fixed-priority with preemption algorithm OR

        -   A lower priority Q is not scheduled or is preempted if there
            are any processes in a higher priority Q

    -   A weighted round-robin algorithm.

        -   Each Q is given a **weighted **time quanta to schedule its
            own processes

-   What advantage is there in having different time quantums at
    different levels of a multilevel queueing system?

    -   Processes that need more frequent servicing, for instance,
        interactive processes such as editors, can be in a queue with a
        small time quantum. Processes with no need for frequent
        servicing can be in a queue with a larger quantum, requiring
        fewer context switches to complete the processing, and thus
        making more efficient use of the computer.

<span id="anchor-5"></span>Multilevel feedback Q scheduling(Figure 16)

-   Similar to multilevel Q scheduling.** Process move between Q's
    though**

-   In addition to number of Q's and scheduling algorithm for each a
    Multilevel feedback Q, scheduler is defined by the following
    parameters

    -   The method used to determine when to upgrade a process to a
        higher priority Q. This is to prevent starvation of processes in
        a lower priority Q

    -   The method used to determine when to downgrade a process to a
        lower priority Q

        -   Example: When a process does not finish its CPU burst in the
            quanta of time alloted to process of that Q. This means that
            this is not an interactive or an I/O bound process and hence
            the downgrade.

-   OS's that employ this scheduler - Solaris

<span id="anchor-6"></span>Highest response ratio first

-   Like SJFS, the next burst time of the process must be known
    before-handed
-   It is a non-preemptive algorithm
-   With SJFS, only the burst time is taken into account ensuring that
    processes with short burst times get executed first. A process with
    a large burst time(a cpu intensive job), could thus have a very
    large response as well as turnaround time.
-   HRRF or HRRN ensures that the waiting time of a process is taken
    into account, i.e., it implements a form of aging
-   Every jobs response ratio is calculated, T<sub>R</sub> = = 1 + , and
    the job with the highest response ratio is scheduled or dispatched.
    W = Waiting time, S = Service time or burst time
-   The response ratio gives a higher priority not only for processes
    with a short burst(due to a small S) but also to processes who have
    been waiting for long(Due to a large W).
