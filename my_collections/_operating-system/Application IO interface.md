---
title: Application IO interface
---

# Synchronous IO with Blocking system calls
- When an application issues a blocking system call, the execution of the application is suspended.
- The application is moved from the operating system's tun queue to a wait queue.
- After the system call completes. The application is moved back to the run queue, where is is eligible to resume execution. When it resumed execution, it will have received the values returned by the system call.
- The actual physical actions performed by IO devices are generally aysnchronous – they take a varying or unpredictable amoung of time. Nevertheless, most operating systems use blocking system calls for application interface, because blocking application code is easy to understand than nonblocking application code.

# Nonblocking system calls

- A nonblocking system call does not halt the execution of the application for an extended time. Instead, it returns quickly, with a return value that indicates how many bytes were transferred.
- Applications needing nonblocking IO can either use nonblocking calls if the operating system provides them or the application writer can overlap execution with IO by creating a multithreaded application. Some threads perform blocking system calls while others continue exciting.
- Applications of the later form are
  - Text editors which receives keyboard and mouse input while processing and displaying data on the screen.
  - Video application which reads frames from a file on a disk while simultaneously decompressing and displaying the output on the display.

# Asynchronous system call

- An asynchronous call returns immediately, without waiting for the IO to complete.
- The application continues to executer its code. The completion of the IO at some future time is communicated to the application, either through
  - Setting of some variable in the address space of the application or
  - The triggering of a signal or software interrupt or a call-back routine that is executed outside the linear control flow of the application.
- The difference between nonblocking and asynchronous system calls is that a nonblocking read() returns immediately with whatever data are available – the full number of bytes requested, fewer or none at all. An asynchronous read() call requests a transfer that will be performed in its entirity but will complee at some future time.
