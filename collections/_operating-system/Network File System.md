---
title: Network file system - Incomplete
---
-   Is a client-server based file system. NFS helps integrate the server
    file system with the directory structure and interface of the client
    file-system. It is an implementation and specification of a software
    system for accessing remote files across LANS.
-   Uses TCP/IP on LANS and sometimes on WANS too for communication.
    Servers offer remote file services to clients using RPC mechanisms.

I.e a client wanting to do an operation(read/write) on a mounted
file-system will be making a RPC call transparently to the servers whose
file-system is mounted on the client. The RPC calls and replies
themselves are done using TCP/IP.
