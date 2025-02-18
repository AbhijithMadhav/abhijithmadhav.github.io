---
title: File system implementation
---
Volume

An entity(A single storage area) containing the file system. It may be a
part of a disk, a whole disk or a combination of parts of multiple disks
and whole disks together.

Partition

Refers to a separate and distinguishable physical part of a hard disk. A
partition may or may not contain a volume. i.e., a volume may span
multiple partitions.

Root partition or primary partition

A partition containing a kernel.

Extended partition

Partitions which cannot contain file systems themselves. They may be
divided into logical volumes for this purpose though.

Difference between a volume and a partition

Volumes are separate storage areas only logically which exist at the
logical OS level. Partitions are actual separate storage areas that
exist at the physical disk level.

Logical drives

An equivalent terminology for volumes in Windows

[Important data structures of a file
system](../Important%20data%20structures%20of%20a%20file%20system.odt)

-   On-disk file system data structures

    -   [Boot Control
        Block](../Important%20data%20structures%20of%20a%20file%20system.odt#Boot%20Control%20Block)
    -   [Volume Control
        Block](../Important%20data%20structures%20of%20a%20file%20system.odt#Volume%20Control%20Block)
    -   [Directory
        Structures](../Important%20data%20structures%20of%20a%20file%20system.odt#Directory%20Structures)
    -   [File](../Important%20data%20structures%20of%20a%20file%20system.odt#File)[
        Control
        Block](../Important%20data%20structures%20of%20a%20file%20system.odt#FCB)

-   In-memory file system data structures

    Is used for both file-system management and improvement of
    performance via caching

    -   [Mount
        Table](../Important%20data%20structures%20of%20a%20file%20system.odt#Mount%20Table)
    -   [Directory-Structure
        Cache](../Important%20data%20structures%20of%20a%20file%20system.odt#%23Directory-Structure%20Cache)
    -   [System-wide open-file
        table](../Important%20data%20structures%20of%20a%20file%20system.odt#%23System-wide%20open-file%20table)
    -   [Per-process open-file
        table](../Important%20data%20structures%20of%20a%20file%20system.odt#%23Per-process%20open-file%20table)
    -   [Buffers](../Important%20data%20structures%20of%20a%20file%20system.odt#%23Buffers)

Through the layers

-   Creation of a file-system

    -   Application calls the logical file system with the pnemonic for
        the filename and the location
    -   Logical file system knows the format of the directory
        structures. It creates a new file, it allocates a new FCB
    -   The system then reads

Internal file structure

Access methods
