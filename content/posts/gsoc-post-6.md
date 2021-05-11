+++
draft = false
date = 2018-07-24T19:03:31+05:30
title = "GSoC Blog Post 6"
slug = "gcoc-post-6"
tags = ["gsoc", "psf", "mercurial"]
categories = ["Tech","GSoC"]
+++

# GSoC Blog post #6

At the beginning of this week, my previous commit was revert and set to config = False, meaning that it is hidden behind that config option. That’s because it was a period of code freeze in mercurial as the next release is due in some days, so unless a feature is completely tested it can’t be released.

Link to the revision: https://phab.mercurial-scm.org/D3919

Next Yuya told me to work on passing multiple revisions with `–all-files` mode.

I have had some code developments related to it posted on `BitBucket`.

The work is currently in progress at:
https://bitbucket.org/sangeet259/mercurial-scm/commits/e0c1548a16e1b47effdbab14a1856122ad39cd57

This patch needs to handle some edge cases which I am trying to figure out like making it compatible with –diff flag, also this needed to be backed up some testing, which I am writing as of now. This is the final leg of my proposal. Rest remaining is documentation and writing more steps.
