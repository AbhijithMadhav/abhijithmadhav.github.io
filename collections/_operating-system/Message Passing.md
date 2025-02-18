---
title: Message passing(Figure 6)
---
-   A message is sent from the the sender to the receiver without
    sharing the address space of the sender to the receiver.
-   Senders and receivers can synchronise themselves too by passing
    messages.
-   Message passing is particularly useful in a distributed environment
    where processes reside in different computers and are connected via
    a network.

Design considerations for applications seeking to use message-passing
and choices offered by the message-passing infrastructure of the
operating system

|                                                                                                                           |                                                                               |
|---------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------|
| Design consideration for applications using message-passing                                                               | Choices offered by the message-passing infrastructure of the operating system |
| [Nature of ](#Nature of logical link)[**logical**](#Nature of logical link)[ communication link](#Nature of logical link) | Direct or indirect                                                            |
| [Type of communication](#Type of communication)                                                                           | Synchronous or Asynchronous                                                   |
| [Type of buffering](#Type of buffering)                                                                                   | No-Buffering, Automatic or explicit                                           |
| [Message sizes](#Message sizes)                                                                                           | Fixed-sized or Variable-sized                                                 |

<span id="anchor"></span>Implication of choosing between direct or
indirect communication

<table>
<tbody>
<tr class="odd">
<td>Direct communication</td>
<td>Indirect communication</td>
</tr>
<tr class="even">
<td>Two processes wishing to communicate must know each others identity. Link is established between every two processes that want to communicate</td>
<td>Two processes wishing to communicate must have a shared postbox. Link is established between two processes only if they share a common postbox</td>
</tr>
<tr class="odd">
<td>Link is associated with exactly two processes</td>
<td>Link is associated with more than two processes; With every process that shares the postbox.</td>
</tr>
<tr class="even">
<td>Between each pair of processes, there exists only link.</td>
<td>Between each pair of communicating processes, there may be a number of different links, with each link corresponding to one mailbox.</td>
</tr>
<tr class="odd">
<td>Changing the identity of one a process may necessitate examining all references to it by other communicating processes.</td>
<td>No such issue. 'send' and receive primitives are addressed using mailboxes and not process id's.</td>
</tr>
<tr class="even">
<td><p>Primitives supported:</p>
<p><span class="math display"><em>s</em><em>e</em><em>n</em><em>d</em><em>T</em><em>o</em>(<em>r</em><em>e</em><em>c</em><em>i</em><em>e</em><em>v</em><em>e</em><em>r</em> − <em>i</em><em>d</em>,<em>m</em><em>e</em><em>s</em><em>s</em><em>a</em><em>g</em><em>e</em>)</span></p>
<p><span class="math display"><em>r</em><em>e</em><em>c</em><em>e</em><em>i</em><em>v</em><em>e</em><em>F</em><em>r</em><em>o</em><em>m</em>(<em>s</em><em>e</em><em>n</em><em>d</em><em>e</em><em>r</em> − <em>i</em><em>d</em>,<em>m</em><em>e</em><em>s</em><em>s</em><em>a</em><em>g</em><em>e</em>)</span></p></td>
<td><p>Primitives supported:</p>
<p><span class="math display"><em>s</em><em>e</em><em>n</em><em>d</em><em>T</em><em>o</em>(<em>M</em><em>a</em><em>i</em><em>l</em><em>B</em><em>o</em><em>x</em>,<em>m</em><em>e</em><em>s</em><em>s</em><em>a</em><em>g</em><em>e</em>)</span></p>
<p><span class="math display"><em>r</em><em>e</em><em>c</em><em>e</em><em>i</em><em>v</em><em>e</em><em>F</em><em>r</em><em>o</em><em>m</em>(<em>M</em><em>a</em><em>i</em><em>l</em><em>B</em><em>o</em><em>x</em>,<em>m</em><em>e</em><em>s</em><em>s</em><em>a</em><em>g</em><em>e</em>)</span></p></td>
</tr>
</tbody>
</table>

Implications of the location of mailbox on the receiving privileges in
*indirect* communication

|                                                                                                      |                                                                                                                                                |
|------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------|
| Location of mailbox                                                                                  |                                                                                                                                                |
| User space                                                                                           | Kernel space                                                                                                                                   |
| Mailbox has unique owner. No confusion regarding who should receive the message sent to the mailbox. | Mailbox has an existence of its own. Ownership and receive privileges may be passed on and thus may change during the lifetime of the mailbox. |
| Mailbox disappears when user process terminates                                                      | A process having the appropriate privilege must explicitly terminate the mailbox through a system call                                         |

<span id="anchor-1"></span>Implication of choosing between message sizes
in message passing

<table>
<tbody>
<tr class="odd">
<td></td>
<td>Implementation perspective</td>
<td>Buffering perspective</td>
<td><p>Consider how Windows 2000</p>
<p>handles this situation: with fixed-sized messages (anything &lt; 256</p>
<p>bytes), the messages are copied from the address space of the</p>
<p>sender to the address space of the receiving process. Larger mes-</p>
<p>sages (i.e. variable-sized messages) use shared memory to pass</p>
<p>the message.</p></td>
</tr>
<tr class="even">
<td>Fixed-sized messages</td>
<td>System Implementation is easy. Application programming is Difficult.</td>
<td><p>A buffer with a specific size can hold a known number</p>
<p>of messages</p></td>
<td></td>
</tr>
<tr class="odd">
<td>Variable sized messages</td>
<td>System Implementation is difficult. Application programming is easy.</td>
<td>Number of messages that can be held is unknown. Will block or return error unexpectedly</td>
<td></td>
</tr>
</tbody>
</table>

<span id="anchor-2"></span>Implications of choosing between various
buffering schemes in message passing

|                                 |                                                    |                                                                 |                     |
|---------------------------------|----------------------------------------------------|-----------------------------------------------------------------|---------------------|
|                                 | No-Buffering                                       | Automatic Buffering                                             |                     |
| Bounded                         | Unbounded                                          |                                                                 |                     |
| Blocking send(Synchronous)      | Sender blocks until recipient receives the message | Sender blocks until recipient receives the message if Q is full | Sender never blocks |
| Non-Blocking send(Asynchronous) | Sender never blocks                                | Sender never blocks                                             | Sender never blocks |

Automatic Buffering vs. Explicit buffering

There are no specifications on how automatic buffering will be provided;
one scheme may reserve sufficiently large memory where much of the
memory is wasted. Explicit buffering specifies how large the buffer is.
In this situation, the sender may be blocked while waiting for available
space in the queue. However, it is less likely memory will be wasted
with explicit buffering.

<span id="anchor-3"></span>Implications of choosing between Synchronous
or Asynchronous message passing

|                                                                                                                                                                                                                                                     |                                                 |                                        |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------|----------------------------------------|
|                                                                                                                                                                                                                                                     | Sender                                          | Receiver                               |
| Synchronous                                                                                                                                                                                                                                         | Blocks until the recipient receives the message | Blocks until message is received       |
| Asynchronous                                                                                                                                                                                                                                        | Sends message and resumes operations            | Receives a valid message or no-message |
| Different combinations of the send() and receive() are possible. If send() and receive() are both blocking, there is a rendezvous between the sender and receiver. The solution to the producer-consumer problem would be trivial in this scenario. |                                                 |                                        |

Advantages and Disadvantages of synchronous and asynchronous
communication

A benefit of symmetric communication is that it allows a rendezvous
between the sender and receiver which would allow easy solution to
producer-consumer like problems.

A disadvantage of a blocking send is that a rendezvous may not be
required and the message could be delivered asynchronously; received at
a point of no interest to the sender(Example: ???). As a result,
message-passing systems often provide both forms of synchronization.
