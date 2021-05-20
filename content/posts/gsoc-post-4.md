+++
draft = false
date = 2018-06-27T19:03:31+05:30
title = "GSoC Blog Post 4"
slug = "gcoc-post-4"
tags = ["gsoc", "psf", "mercurial"]
categories = ["Tech","GSoC"]
+++

# GSoC Blog Post #4

Another Fortnight gone by, and perhaps this was the time when we got most work done.

This time around I added two new features to mercurial.

They are: The allfiles mode

This would work on a single revision and get all the files that were
there in that revision. So it’s like grepping on a previous state.
Using this with wdir() :: hg grep -r "wdir()" --allfiles is what the
default behaviour is desired for grep.

Link to PR : https://phab.mercurial-scm.org/D3728

Then I also added the diff mode: This is a duplicate of the exisitng all flag. Since --all searches diffs, there diff is a better name for it.
The --all flag is still here for backward compatibility reasons.

Link to PR : https://phab.mercurial-scm.org/D3763

I am working on two more patches right now, will be posting about that in the next blog.
