---
title: Pipes
---
Pipes are an implementation of a message passing system.

Considerations

1.  Full or Half duplex – Communication in both directions at the same
    time.
2.  Must a relationship exist between the processes
3.  Can they be used to communicate over a network

Types

1.  Unnamed – Ex: Unix pipes
2.  Named – Ex: Unix FIFO pipes

Unix unnamed pipes(Figure 8)

1.  Half duplex and unidirectional
2.  Can be used only by processes sharing the same ancestor as both the
    processes need to have access to both ends of the pipe. This is
    possible by a parent-child pair. The pipe created by the parent can
    be accessed by the child via the inherited file descriptors.
3.  It is a special kind of file existing in the kernel address space.
    This file is not accessible through the file system.
4.  Persists only as long as the program is running

FIFO pipes(Figure 9 and 10)

1.  Full duplex and bidirectional though they are used is unidirectional
    IPC's
2.  Any **two or more** processes can communicate. No need for common
    ancestry.
3.  Created using

-   -   int mkfifo(const char \*pathname, mode_t mode)

1.  FIFO's do not invent a new namespace and can be accessed using
    ordinary filesystem based system-calls(open, close, write).

2.  Typical uses

    1.  Used by shell commands to pass data from one pipeline to the
        other without creating a temporary file
    2.  Used a rendezvous points in client-server applications to pass
        data between server and clients.
