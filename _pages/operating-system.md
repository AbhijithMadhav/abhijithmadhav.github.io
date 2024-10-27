---
title: Operating System
layout: collection
permalink: /operating-system/
collection: operating-system
classes: wide
author_profile: false
---

## Reference
Most of the material here is a synthesis of 
- Silberscatz
- Tenaunbaum
- Richard Stevens

Original creation date : Around 2010-2012

## Contents

-   [Processes](Processes)
    -   [Context Switching](Context%20Switching)
    -   [Threads](Threads)
    -   [CPU scheduling](Process%20Scheduling)
    -   [Signals](Signals)
-   Inter-process communication
    -   [Shared Memory](Shared%20Memory) - [Inspecting SHM Processes](InspectingSHMProcesses.txt)
    -   [Message Passing](Message%20Passing) -
        [Pipes](Pipes)
-   [Synchronization and concurrency among Processes](Process%20synchronization)
    -   [Locks](Locks)
    -   [Atomic Hardware primitives](Atomic%20hardware-primitives%20for%20synchronization)
    -   [Semaphores](Semaphores)
        -   [Bounded buffer using counting semaphores](workspace/Bounded%20Buffer%20using%20counting%20semaphores/main.c)(Figure
            23)
        -   Readers-Writers using counting-semaphores(Figure 24)
        -   [Dining Philosophers problem using semaphores](workspace/Dining%20Philosophers%20using%20semaphores/main.c)(Figure
            25)
    -   [Monitors](Monitors)
        -   [Bounded buffer using condition variables](workspace/Bounded%20Buffer%20using%20condition%20variables/main.c)
        -   [Bounded buffer using monitors](workspace/Bounded%20buffer%20using%20monitors/src/BoundedBuffer.java)
        -   [Dining Philosophers problem using condition-variables](workspace/Dining%20Philosophers%20using%20condition%20variables/main.c)
-   [Deadlock](Deadlocks)
    -   [Deadlock prevention](Deadlock%20Prevention)
    -   [Deadlock avoidance](Deadlock%20Avoidance)
    -   [Deadlock](Deadlock%20detection%20and%20recovery) : [detection and](Deadlock%20detection%20and%20recovery)[recovery](Deadlock%20detection%20and%20recovery)
    -   [Handling deadlocks in the concurrency control component of database systems](Databases/Handling%20deadlocks%20in%20concurrency%20control%20systems)
-   Memory management and virtual memory
    -   [Memory management](Memory%20management)
        -   [Contiguous memory allocation schemes](Contiguous%20Memory%20Allocation%20Scheme)
        -   [Paging](Computer%20Organization%20and%20Architecture/Virtual%20Memory)
        -   [Segmentation](Segmentation)
    -   [Virtual memory](../Computer%20Organization%20and%20Architecture/Virtual%20Memory)
        -   [Structure of Page Tables](../Computer%20Organization%20and%20Architecture/Virtual%20Memory%20-%20Structure%20of%20Page%20Tables)
        -   [Page Replacement](../Computer%20Organization%20and%20Architecture/Virtual%20Memory%20-%20Page%20Replacement)
        -   [Frame allocation](../Computer%20Organization%20and%20Architecture/Virtual%20Memory%20-%20Frame%20allocation)
        -   [Memory usage of Page tables](../Computer%20Organization%20and%20Architecture/Virtual%20Memory%20-%20Memory%20usage%20of%20Page%20tables)
        -   [Translation Lookaside buffer(TLB)](../Computer%20Organization%20and%20Architecture/Virtual%20Memory%20-%20Translation-Lookaside%20buffer(TLB))
        -   [Design issues](../Computer%20Organization%20and%20Architecture/Virtual%20Memory%20-%20Design%20issues)
        -   [Thrashing](../Computer%20Organization%20and%20Architecture/Virtual%20Memory%20-%20Thrashing)
        -   [Memory Mapped Files](../Computer%20Organization%20and%20Architecture/Virtual%20Memory%20-%20Memory%20Mapped%20Files)
-   File system
    -   [Binary Files](Binary%20Files)
    -   [File system structure](File%20System%20Structure)
    -   [File system implementation](File%20System%20Implementation)
        -   [Important data structures of a file system](Important%20data%20structures%20of%20a%20file%20system)
        -   [Directory Structure Implementation](Directory%20implementation)
        -   [File Allocation Methods](File%20Allocation%20Methods)
        -   [Free Space Management](Free%20Space%20Management)
        -   [Design considerations of a file system](Design%20considerations%20of%20a%20file%20system)
        -   [File System Recovery](File%20System%20Recovery)
        -   File System Backup
        -   [NFS](Network%20File%20System)
-   IO
    -   [Application IO interface](Application%20IO%20interface)
-   Protection and security


