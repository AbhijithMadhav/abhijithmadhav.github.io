---
title: Binary files
---

ASCII or text files aren't processed using 'C'. Perl and shell scripts
provide far more powerful constructs and tools to do this job.

That being said, binary files can be handled in 'C' via fread and
fwrite. As binary files are far more widely used to store data than text
files, knowing to process binary files in 'C' is important.

How are binary files more efficient than ASCII files

Because fread and fwrite can be designed for them which can read in data
in quantities they like

For ASCII files only printf, scanf and gets can be designed
