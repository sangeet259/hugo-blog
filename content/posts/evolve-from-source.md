+++
draft = false
date = 2019-01-22
title = "Using Evolve from source"
slug = "evolve-from-source"
tags = ["devops","mercurial","extensions","octobus"]
categories = [ "Blog"]
+++

I was working on `Evolve`. I made some changes to it's codebase, at this point it becomes very important to me to see the effect of the changes that I make, because unlike others who just trust their code and directly run tests on the code.

**I don't run tests unless I see the working of the code I write.**

Doing this for Mercurial repo was easier, I didn't have to do much.
A simple `./hg <command>` was sufficient.

To do this for `Evolve` or any other Mercurial Extension you develop on, do this :

1. Clone Evolve : `hg clone https://www.mercurial-scm.org/repo/evolve/`
2. **Let's say** the repo is at `/home/sangeet/evolve/`
3. Next go to your hgrc file (it's your wish if you go for the global hgrc or any local hgrc in some other hg repo)  (I go for the global one)
4. Add this: 
```
[extensions]
evolve = /home/sangeet/Octobus/evolve/hgext3rd/evolve
```
**Note** : Please DO NOT put the path in quotes, doing so will give you import errors !


Now you can make changes to evolve at /home/sangeet/Octobus/evolve/hgext3rd/evolve.

The changes that you do there will be reflected everywhere you use any `hg evolve` command.

   
