+++
draft = true
date = 2019-02-16
title = "No-Op context manager in Python"
slug = "no-op-ctx-mgr-in-python"
tags = ["python"]
categories = [ "TIL"]
+++

I had a sent a PR to Evolve which was very minimal, but it involved adding an indentation and a context manager, which made the diff look rather ugly.

Pierre-Yyves suggested me to split the patch into two, one doing just the indentation and the other adding that context manager.

Now when you have to "just" indent,to make way for the context manager, you need to use a no-op context manager.

