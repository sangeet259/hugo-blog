+++
draft = false
date = 2018-07-09T19:03:31+05:30
title = "GSoC Blog Post 5"
slug = "gcoc-post-5"
tags = ["gsoc", "psf", "mercurial"]
categories = ["Tech","GSoC"]
+++

# GSOC Blog Post #5

This time I was working on changing the default of the grep command to a new behaviour.

With this patch grep searches on the working directory by default
and looks for all files tracked by the working directory and greps on them

==========================Demonstration================

**OLD BEHAVIOUR**

```bash
$ hg init a
$ cd a
$ echo "some text">>file1
$ hg add file1
$ hg commit -m "adds file1"
$ hg mv file1 file2
$ hg grep "some"
file2:1:some text
file1:0:some text
```

This behaviour is undesirable since file1 is not in the current history and was
renamed as file2, so the second result was redundant and confusing

**NEW BEHAVIOUR**

```bash
$ hg init a
$ cd a
$ echo "some text">>file1
$ hg add file1
$ hg commit -m "adds file1"
$ hg mv file1 file2
$ hg grep "some"
file2:2147483647:some text
```

======================================================

The change can be seen in the line of the respective snippets, in the second case file1 is no more present in the repo and so is not searched again.

Link to the Differential Revision:: https://phab.mercurial-scm.org/D3826

I then wrote the test demonstrating the same, next I plan to enable passing revs and multiple revisions to grep.
