---
title: File System Recovery
---
A disk crash can cause inconsistencies among on-disk file-system data
structures in addition to the actual corruption of data. Different
approaches to file system recovery are listed below

Consistency checking

-   The file system runs a consistency checker which scans all its
    metadata to confirm or deny the consistency of the system.

-   A file system can record its state within the file-system metadata
    at the start of any metadata change as one of flux. The state is
    changed to stable upon the successful completion of the metadata
    operation. The consistency checker is run only if the state is in
    flux.

-   The consistency checker itself compares the data in the directory
    structure with the data blocks on disk and tries to fix any
    inconsistencies it finds. The allocation and free-space-management
    algorithms dictate what types of problems the checker can find and
    how successful it will be in fixing them.

    -   If linked allocation is used the entire file can be
        reconstructed from the data blocks and the directory structure
        can be recreated.
    -   The loss of a directory entry cannot be recreated if an indexed
        file system is used as data blocks have no knowledge of each
        other.

-   Examples of consistency checkers

-   Disadvantages

    -   Inconsistencies might be unrepairable.
    -   Consistency checking can require human intervention to resolve
        conflicts.
    -   Consistency checks take system and clock time

Log structured file-system or journaling file system

-   All changes to file system structures are written sequentially to a
    log. A set of operations performing a specific task is a
    transaction.
-   Once such a set of operations are written to this log, they are
    considered to be committed, and the system call can return to the
    user process.
-   Parallely these log file entries are replayed across the actual file
    system data structures, thus updating them.
-   As the actual data structures are updated, a pointer pointing to log
    entries is updated to indicate which actions are complete and which
    are incomplete. When an entire committed transaction is completed,
    it is removed from the log file, which is actually a circular
    buffer. The log may be in a separate section of the file system or
    even under a separate disk spindle. It is more efficient to have it
    under separate read and write heads, thereby decreasing head
    contention and seek times.
-   If the file system crashes, the log file will show all committed but
    not completed to the file-system transactions. They can now be
    completed, thus maintaining the sanctity of the file-system data
    structures
-   **Advantage** : Metadata updates proceed much faster then when they
    are applied directly to the on-disk data structures. The reason for
    the improvements is the sequential I/O of writing to the log file of
    the log-structured filesystem compared to the random synchronous
    writes to the disk. The less costly synchronous sequential writes
    are later replayed asynchronously via random writes to the
    appropriate structures.
-   Examples of log-structured file systems – NTFS, Veritas file system

Explain why logging metadata updates ensures recovery of a file system
after a file system crash.

For a file system to be recoverable after a crash, it must be consistent
or must be able to be made consistent. Therefore, we have to prove that
logging metadata updates keeps the file system in a consistent or
able-to-be-consistent state. For a file system to become inconsistent,
the metadata must be written incompletely or in the wrong order to the
file system data structures. With metadata logging, the writes are made
to a sequential log. The complete transaction is written there before it
is moved to the file system structures. If the system crashes during
file system data updates, the updates can be completed based on the
information in the log. Thus, logging ensures that file system changes
are made completely (either before or after a crash). The order of the
changes are guaranteed to be correct because of the sequential writes to
the log. If a change was made incompletely to the log, it is discarded,
with no changes made to the file system structures. Therefore, the
structures are either consistent or can be trivially made consistent via
metadata logging replay.
