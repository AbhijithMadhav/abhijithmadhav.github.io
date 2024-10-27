---
title: Segmentation
---
-   This memory management technique supports a programmer view of
    memory which reflects the programs themselves, i.e.,

    -   The logical address space of a program is consists of a
        collection of segments, say the data segment, the code segment,
        stack segment, heap segment, global variables segment etc
    -   The programmer therefore specifies each address by the tuple,
        \<segment-name, offset>

How does the logical address-space of a process get composed with
segments?

A user program itself defines segments. When it is compiled, the
compiler constructs these segments to reflect the input program.

Working of an MMU based on segmentation

-   Contains a hardware segmentation table
