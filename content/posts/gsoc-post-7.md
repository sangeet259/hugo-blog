+++
draft = false
date = 2018-08-08T19:03:31+05:30
title = "GSoC Blog Post 7"
slug = "gcoc-post-7"
tags = ["gsoc", "psf", "mercurial"]
categories = ["Tech","GSoC"]
+++

# GSoC Blog post #7

This week I worked on passing multiple revisions to the –allfiles flag.

The PR can be found at https://phab.mercurial-scm.org/D3976

It’s assumed that if you are passing multiple revisions to –allfiles,
you want hits from all of them. This was the behaviour that was chose after a discussion with Yuya.

The initial patch that I had sent had a different behaviour.

It made sure it just check the files only once by maintaining a dictionary called files. If there is a file that was used to be tracked by the repo but was subsequently deleted in a revision, and that revision falls between your specified range of revisions, you would get results from that file too.

This completes the overhaul of grep. There is one more thing that is left is :

`hg grep --all-files -rREVS FILE` to scan unchanged revisions?

Although this marks the end of the GSoC coding period, my association with `Mercurial` shall continue for the times to come.
