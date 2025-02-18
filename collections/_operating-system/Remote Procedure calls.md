---
title: Remote procedure calls
---
Is a strategy for communication in a client-server system. It makes use
of message-passing facilities underneath. A client can call and execute
a remote procedure whose address space is on another machine as a local
procedure.

Procedure

RPC system provides a client stub for each call. When the application on
the client side is compiled it is binded with this stub. When the remote
procedure is invoked by the client, the following happens

1.  The stub marshals the arguments. i.e. Converts them to a machine
    independent form(Ex: XDR).
2.  Contacts to a well known master server called the matchmaker and
    asks for the location (port number and address) which would service
    the RPC.
3.  Match-maker replies with the location
4.  Client sends the RPC request to this location.
5.  Server executes the call and returns the result.

RPC semantics

1.  At-most-once – Server can keep a record of the time-stamp and ensure
    that only one execution is done.
2.  Exactly-once – Server sends ack to client as soon as it receives a
    request. This eliminates the possibility of a server not receiving
    the request. The client can resend the request if the ack is not
    received. The server maintains the time-stamp to ensure at-most-once
    execution from its side. The client ensures the exactly-once
    semantics. If the client makes sure of sending requests after
    specific time-outs, the semantics would work even if ACK's from the
    server get lost.

Undesirable consequences of not enforcing at-most-once or exactly-once
semantics

Remote procedure may be invoked more than once. If this RPC call was
meant to represent a withdrawal of money from a bank account, multiple
withdrawals would happen.

What type of operations could afford not to enforce at least one of the
two semantics

Operations which do not alter data or provide time-sensitive results
could afford to not enforce at least one of the two semantics. Ex; Query
information about account balance

Software which uses RPC

-   NFS
