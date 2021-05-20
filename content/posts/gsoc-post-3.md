+++
draft = false
date = 2018-06-11T19:03:31+05:30
title = "GSoC Blog Post 3"
slug = "gcoc-post-3"
tags = ["gsoc", "psf", "mercurial"]
categories = ["Tech","GSoC"]
+++

# GSoC Blog Post 3

These two weeks I was involved in fixing the `–wdir()`. Yuya had asked me to not create an entire separate if/else block as that might lead to untested code paths.

So I was trying to work out another way. While I was trying to figure out a way to do this I realised the code had changed a bit since my last patch, and it nows throws WdirUnsupported exception. This made my life a bit easier.

After consulting Yuya Nishihara, I caught all the WdirUnsupported exceptions and then treated the changeset objects, file node objects differently that would work.

The patch to this can be found at https://phab.mercurial-scm.org/D3673.

Currently, I am working on adding a mode to grep on unmodified files in a changeset.

More on this on my next post, stay tuned.
