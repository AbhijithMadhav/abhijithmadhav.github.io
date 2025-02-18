---
title: Important data structures of a file system
---
<span id="anchor"></span>Boot control block

-   A per volume and hence a per file system data structure

-   A on-disk data structure

-   Contains information needed by the system to boot an OS from that
    volume. Empty if no OS present on that volume.

-   It is in the first block of that volume.

-   Called

    -   Boot block in UFS(Unix file system)
    -   Partition boot sector in NTFS

<span id="anchor-1"></span>Volume control block

-   A per volume data structure

-   Contains volume related book keeping info

    -   Number of blocks
    -   Size of blocks
    -   free block count
    -   free block pointers
    -   Free FCB count
    -   FCB pointers

-   Called

    -   Super block in UFS
    -   Master file table in NTFS

<span id="anchor-2"></span>Directory structures

-   A per file system data structure
-   Used to organize files – Contains filenames and fields pointing to
    data blocks containing the files. In UFS, one instead has the inode
    number which uniquely identifies a FCB which contains pointers to
    the data blocks.
-   In UFS, this includes file names and associated inode numbers
-   In NTFS, this structure is stored in the master file table.

<span id="anchor-3"></span>File control blocks(FCB's, also known as
inodes in Unix systems)

A per file entity. Contains information about the file including

-   Inode number(A unique identifier to allow association of a file
    within a directory entry)
-   File name
-   Permissions
-   Ownership details
-   Time stamps
-   Size
-   Pointers to file data blocks.

<span id="anchor-4"></span>Mount table

Contains informations about each mounted volume.

<span id="anchor-5"></span>Directory-structure cache

Contains the directory information of recently accessed directories.

<span id="anchor-6"></span>System-wide open-file table

Contains copy of FCB of each open file

<span id="anchor-7"></span>Per-process open-file table

Contains a pointer to the appropriate entry in the system-wide open-file
table

<span id="anchor-8"></span>Buffers

Holds file-system blocks when they are being read from disk or written
to disk
