---
title: Monitors
---
Monitors(Figure 29)

-   Are programming constructs or paradigms that aid in synchronization.
    They are at a higher level of abstraction than semaphores.
-   In case a monitor is implemented as a programming construct, it is
    an ADT which ensures that a set of programmer defined operations are
    provided mutual exclusion within the monitor. The compiler takes
    care of generating code such that execution sequences inside any
    method of a monitor is mutually exclusive with execution sequences
    inside another method of the same monitor. One can thus turn all
    critical regions into monitor procedures to avoid having two
    processes/threads execute in the same critical-section. As the
    compiler generates the mutual exclusion code chances of timing
    errors are eliminated.
-   Monitors contain a lock and condition-variables, along with two
    operations on them, wait and signal which help in blocking of an
    execution sequence. These condition variables are used to implement
    scheduling constraints.

Won't two execution sequences be present in a single monitor after a
process/thread has signaled another one?

No. Various schemes are employed to prevent this.

-   Mesa scheduling(signal-style monitors) – Newly awakened process runs
    and the one signaling suspends, i.e., the signaling process places
    itself on the wait Q. Implemented in pthreads
-   Hoare scheduling(notify-style monitors) - Waiting process runs only
    after the notifying process exits, i.e., a notifying on a
    condition-variable merely places the waiting process on the ready Q.
-   Brinch Hanesen's scheduling – Process doing a signal must exit the
    monitor.

How would solutions to synchronization problems differ while using
signal-style and notify-style monitors?

A thread suspending itself on a notify-style monitor should not enter
the CS right after it is awakened. This is because other threads
awakened by this notification(notifyall) might also enter the CS. It
should test the condition-variable once again before entering the CS. In
other words it wait on the condition-variable on a while loop.

In signal style monitors, the awakened thread can go right into the the
CS as the signalling thread would have suspended itself and this is the
only thread woken up

Difference between condition-variables and counting-semaphores

-   Operation of signal() and V()(up()) -

    -   With a semaphore, the signal(V() or up()) operation actually
        increments the value of the semaphore. So if there aren't any
        processes blocked on the semaphore, the signal will be
        "remembered" by the increment of the semaphore. A later process
        which attempts to do a P() wouldn't suspend as it sees that
        there is a pending signal for it to not to.
    -   The signal function on a Monitor’s condition variable is
        different. If there are no processes blocked on the condition
        variable then the signal function does nothing. It is not
        remembered. In order to remember "empty signals" you have to use
        some other form of variable. The good part of this is that using
        other variables within a monitor is simple because we can be
        assured that mutual exclusion is being implemented.

-   Operation of wait() and P() -

    -   When a thread goes to sleep waiting on a condition variable it
        does so by atomically releasing the lock and then going to
        sleep(This is possible as a lock is associated with the
        condition variable). This makes it possible to sleep inside the
        CS. The thread when woken up obtains the lock and then proceeds
        through to the rest of the CS. This makes it easier to represent
        the logic of the program as the details of the synchronization
        mechanism is hid from the application code.
    -   A thread which waits on a semaphore cannot release the lock when
        is is put to sleep as it has no clue about the lock held. Thus
        the programmer must ensure that the thread never goes to sleep
        inside the CS when it is holding a lock. This is again scope for
        error.

Implementing monitors with semaphores

mutex = 1; // Ensure mutual exclusion into monitor methods

semaphore highPriority = 0; // Semaphore on which signaling threads will
wait on // after signaling

int highCount = 0; // Number of threads waiting after signaling, i.e.,
suspended after doing a c.signal()

semaphore cSem = 0; // semaphore implementing the condition-variable 'c'

int cCount = 0; // No. of threads waiting on cSem, i.e., no. of threads
suspended on a c.wait()

<table>
<tbody>
<tr class="odd">
<td>Source</td>
<td>Signal style implementation(Hoare scheduling)</td>
<td>Notify style implementation(Mesa scheduling)</td>
</tr>
<tr class="even">
<td>Code for start() at the start of every monitor-method start</td>
<td>mutex.down();</td>
<td>mutex.down();</td>
</tr>
<tr class="odd">
<td>c.wait()</td>
<td><p>cCount++;</p>
<p>if (highCount &gt; 0)</p>
<p>    highPriority.up();</p>
<p>else</p>
<p>    mutex.up();</p>
<p>cSem.down();</p>
<p>cCount--;</p>
<p>Wake up a higher-priority signaler who has been suspended if any. He will release the mutex after her finishes</p>
<p>Else give up mutex, and suspend self</p></td>
<td><p>cCount++;<br />
mutex.up();<br />
cSem.down();<br />
mutex.down();</p>
<p>Release lock before suspending self.</p>
<p>Obtain lock after being awakened.</p></td>
</tr>
<tr class="even">
<td>c.signal()</td>
<td><p>if (cCount &gt; 0) {</p>
<p>    highCount++;</p>
<p>    cSem.up();</p>
<p>    highPriority.down();</p>
<p>    highCount--;</p>
<p>}</p>
<p>Signalers don't signal if there are no suspended threads on a c.wait()</p>
<p>If there are wake them up and suspend self.</p></td>
<td></td>
</tr>
<tr class="odd">
<td>c.notify()</td>
<td></td>
<td><p>if (cCount &gt; 0) {</p>
<p>    cCount--;</p>
<p>    cSem.up();</p>
<p>}</p>
<p>// Only if there are processes waiting for a notify, send a notification and continue.</p></td>
</tr>
<tr class="even">
<td>Code for exit() at every return point of the method</td>
<td><p>if (highCount &gt; 0)</p>
<p>    highPriority.up();</p>
<p>else</p>
<p>    mutex.up();</p>
<p>Wake up the higher priority signalers if any of them are suspended. They will give up the lock to the monitor-object on their way out.</p>
<p>Else release the lock for the monitor object.</p></td>
<td>mutex.up();</td>
</tr>
<tr class="odd">
<td></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>
