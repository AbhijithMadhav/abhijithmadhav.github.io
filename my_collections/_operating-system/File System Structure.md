---
title: File system structure
---
Why are disks the preferred secondary storage media?

-   A disk can be rewritten in place
-   A disk can **directly** access any block of information it contains.
    It is simple to access any file sequentially or randomly

Block

Basic unit of transfer of data between disk and memory. IO transfer is
done in terms of blocks for the sake of efficiency. Each block has one
or more sectors. The size of a sector is varies from 32 to 4k bytes

File system

Provides the structure for storage of data in a secondary device. They
provide efficient and convenient access to the disk by allowing data to
be stored, located and retrieved easily.

Main design issues

-   Defining how the file system should look to the user. This involves
    defining

    -   A file and its attributes,
    -   The operations allowed on a file
    -   The directory structure for organizing files.

-   Creating algorithms and data structures to map the logical file
    system onto the physical secondary storage devices

Device driver?

System software, part of operating system which converts high-level IO
commands like “retrieve block 123” to low level hardware specific
instructions understandable by the specific IO controller. It does this
by writing specific bit patterns to special locations in the IO
controller memory.

Logical layers of the file system

<table>
<tbody>
<tr class="odd">
<td>Layer</td>
<td>Input</td>
<td>Output</td>
<td>Data structure used</td>
</tr>
<tr class="even">
<td>Application</td>
<td>NA</td>
<td>NA</td>
<td>NA</td>
</tr>
<tr class="odd">
<td>Logical file system</td>
<td><p>Mnemonic name of the file and</p>
<p>Operation to be preformed</p></td>
<td><p>Logical file location(Inode number)</p>
<p>Operation to be performed</p></td>
<td>Relevant directory structures</td>
</tr>
<tr class="even">
<td>File organization module</td>
<td><p>Logical file location(Inode number) and</p>
<p>Operation to be performed</p></td>
<td><p>Physical block numbers of the file</p>
<p>Operation to be performed</p></td>
<td>File control block of the particular file</td>
</tr>
<tr class="odd">
<td>Basic file system</td>
<td><p>Physical block numbers of the file and</p>
<p>Operation to be performed</p></td>
<td>Invokes suitable interface of the device driver to perform the operation</td>
<td>NA</td>
</tr>
<tr class="even">
<td>IO control</td>
<td>Handles transfers between disk and memory</td>
<td>NA</td>
<td></td>
</tr>
</tbody>
</table>

Advantages of a layered file system

Duplications of code is minimized. The IO control and sometimes the
basic file system code can be used by multiple file systems. Each of
these file systems have their own file organization module and logical
file system

Disadvantages of layered file system

Can increase operating system overhead resulting in decreased
performance

Virtual file system

-   In systems containing multiple file systems the layered approach
    reduces duplication in the code by enabling different file systems
    to use the same basic file system and implement only the logical and
    file organization module to suit the specific requirements of
    multiple file systems.
-   In practice such a system is not practical as applications would
    need to implement multiple ways of interacting with the different
    file systems.
-   The virtual file system is a layer above the different specific file
    system interfaces which provides a common interface for applications
    to interact with different file system. The different file systems
    may even be remote ones served through NFS.
-   Conceptually the VFS maps the common interface functionality to file
    system specific interfaces for multiple file systems
-   Conceptually VFS introduces a layer of indirection in the file
    system implementation. In many ways, it is similar to
    object-oriented programming techniques. System calls can be made
    generically (independent of file system type). Each file system type
    provides its function calls and data structures to the VFS layer. A
    system call is translated into the proper specific functions for the
    target file system at the VFS layer. The calling program has no
    file-system-specific code, and the upper levels of the system call
    structures likewise are file system-independent. The translation at
    the VFS layer turns these generic calls into file -system-specific
    operations.
