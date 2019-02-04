+++
draft = false
date = 2018-08-12T19:03:31+05:30
title = "GSoC Final Post"
slug = "gcoc-final-post"
tags = ["gsoc", "psf", "mercurial"]
categories = ["Blog","GSoC"]
+++
# HG Grep | The Google Summer of Code Project
## Mercurial | Python Software Foundation

**Mentors** : Yuya Nishihara, Pulkit Goyal, Jordi GH, Kevin Bullock, Sean Farley

I did my project under `Python Software Foundation` and `Mercurial`. My project in the GSoC’18 dealt with the grepping aspect of Mecurial

Now, before diving into it, making some things clear from the title.

### What is Google Summer of Code ?

<center>![gsoc-img](https://cdn-images-1.medium.com/max/800/1*LmIMc8mqJLQTRtFtOKdPVg.png)</center>

**Google Summer of Code** is program to promote open-source development. It is organised by Google every summer since last decade. It runs for 12 weeks from mid May to Mid August. Students contribute to open-source repositories and get paid by Google in return.

### What is Mercurial ?
<center>![](https://cdn-images-1.medium.com/max/800/1*WMxyzi8OyGI-_Yys9_YIPw.png)</center>

<center>**The Mercurial Version Control System**</center>

**Mercurial** is a distributed revision-control tool for software developers. There are other SCMs like Git, SVN, perforce.

### Grep Command
Grep is a command-line utility for searching plain-text data sets for lines that match a regular expression.

### Hg Grep command
Mercurial’s grep gives the power you need to grep on a repository, that is you can also search history, specific versions of specific files and much more. More on hg grep in a later section.

### The Project

> Improvising and Fixing Mercurial’s grep command
The entire proposal can be found at : https://www.bit.ly/gsocprop

There were certain aspects of Mercurial’s grepping system that were either buggy or not what people wanted it to be like.

You can read the entire plan at : https://www.mercurial-scm.org/wiki/GrepPlan

### The all flag problem
Before visiting the problem, let’s first discuss what this flag does/supposed to do :

* Look at diffs, not file contents
* Look at all diffs, not stop at the first hit
* A tool for seeing when a change was introduced (+) or removed (-)

**The problem and the solution**
Issue : https://bz.mercurial-scm.org/show_bug.cgi?id=3885

Everything was fine when you pass the revisions in reverse revlog order, but the `--all` flag gave erroneous output when the revisions are passed in a revlog order , ie bottom to top :: `-r 0:tip`

I debugged into it and after a thorough scrutiny I realised few things:

There are no removals in the output of revlog ordered `grep --all`
Even in case of additions, they are just not correct
I realised this was due to the empty pstate (parent state) dictionary that was being passed. And this dictionary was empty because at the end of each revision we are simply deleting the **matches[rev]**. So in the next revision it’s passing **[]** as `pstate`, as the computation of diff via `difflib.SequenceMatcher()` requires comparing the previous state with this state, it’s simply showing that the previous state is empty and to get the current state, you just need to add everything from the current state to the pstate. This is the reason why there are no removals in the output and the discrepancy even in the additions.

To solve this we need to simply keep the matches and not delete it. But as Jordi told, there will be a huge memory leak which we can not afford, so I came up with another solution of keeping the matches dictionary only till the end of this window and clearing it up once the window ends. I did not make any changes to the line` del revfiles[rev]` . So we will know when a window ends when this revfiles dict gets empty.

Hence this :

```python
del revfiles[rev]
if not revfiles:
  matches.clear()
Thus the issue was fixed.
```

> A more detailed read :
> http://sangeet259.github.io/2018/03/26/demonstrating-the-problem-with-forward-ordered-grep-all-in-mercurial.html
> 
> https://phab.mercurial-scm.org/rHGa2a6755a3defe471113eb9b259dcd8fd5f20566c

**Additional work related to it:**

* adding a --diff flag
* deprecating the --all flag

1. **Adding a diff flag:**
The name all for the flag --all makes no sense at all. There is nothing all about it. So I introduced another flag -all . It works exactly same as `all`. I also added some major tests related to it, which were there for the all flag.

  > https://phab.mercurial-scm.org/rHG7fbb5d76c55598c8d361ce7c70482301f0887cfb

2. **Deprecating the --all flag**

Since `diff` flag was introduced, we should move to diff flag from all, deprecating it in the process.

>https://phab.mercurial-scm.org/rHG7fbb5d76c55598c8d361ce7c70482301f0887cfb

## The changing and fixing of default of hg grep

**The problem :**

This searches all filelogs in reverse revlogs order, via walkchangerevs, and for each filelog outputs the first revision in which the target regexp is found. File contents are searched, not diffs, and all filelogs are searched, not just those in the current manifest.

This behaviour has been in place for a long time, but it’s confusing and not very useful.

One of the most common thing people want to do is grep the current directory, but only files under hg control. Currently, this is inconvenient and at best requires doing something like `hg files -0 | xargs -0grep` in order to accomplish this.

This was an smoother work and I divided the entire work into few parts.

Initially I had sent a big patch, solving the issue. But Yuya suggested that this may not be good approach as we may have some untested code paths due to the huge chuck of if else block that was introduce.

> https://phab.mercurial-scm.org/D2938


Then I split up the work into patches:

* enable passing “wdir()” as a revision to -r flag
* adding a allfiles flag
* adding multirev support to allfiles flag
* adding support for individual files in allfiles mode
* changing the default behaviour


**[1]. Enable passing “wdir()” as a revision to -r flag**
   
   So earlier if you tried passing “wdir()” to the -r flag, it used to give a WdirUnsupported error. With this patch when you pass wdir() to the -r flag, it catches the WdirUnsupported error and falls back to an alternate path.

  So now the files need not be necessarily commited to a changeset in order to be grepped upon. And this was the basic premises of the entire changing of default behaviour.

>https://phab.mercurial-scm.org/rHG16f93a3b8b05aae9391f26a8b908bfa716456757

**[2]. Adding an allfiles flag**

Untill now, you could only grep on the files that were changed in the particular revision. But to search on the current state of all files in the present working directory we need all the files from the passed changeset. This patch made that possible.

>https://phab.mercurial-scm.org/rHGb8f45fc27370dc7df283c47f71927c10462197fb

**[3]. Adding multirev support to allfiles flag**

So, up untill now you could only have a single revision passed along with the --allfiles flag. Here I changed that behaviour, by not skipping files that have been already been seen previously.

>https://phab.mercurial-scm.org/rHGf3f109971359e16ff5ade45821b697329a326183

**[4]. Adding support for individual files in allfiles mode**

Previously (up untill now), suppose you want to search a pattern in a specific file or a specfic set of files, in a specific revision or a set of revisions, there was no way of doing that, if those revisions weren’t responsible for any change in the files specified. But with the coming of allfiles flag, it was possible to have this functionailty. This patch, facilated that.

>https://phab.mercurial-scm.org/rHG9ef10437bb88c27645796b95c6ab78e12c307117

**[5]. Changing the default behaviour**

Here I caught the default state, ie no revisions passed, no diff flag. And then specified the parameters that need to be set, in order to simulate what we want the default to be. Specifically, it should be searching the wdir and it should consider all the files.

A small demo of the behaviour.

<u>**OLD BEHAVIOUR**</u>
```bash
$ hg init a
$ cd a
$ echo “some text”>>file1
$ hg add file1
$ hg commit -m “adds file1”
$ hg mv file1 file2
$ hg grep “some”
file2:1:some text
file1:0:some text
```

This behaviour is undesirable since file1 is not in the current history and was
renamed as file2, so the second result was redundant and confusing.

<u>**NEW BEHAVIOUR**</u>
```bash
$ hg init a
$ cd a
$ echo “some text”>>file1
$ hg add file1
$ hg commit -m “adds file1”
$ hg mv file1 file2
$ hg grep “some”
file2:2147483647:some text
```

>https://phab.mercurial-scm.org/rHG9ef10437bb88c27645796b95c6ab78e12c307117


## Some other works
**I also worked on some other interesting things apart from grep.**

**They were:**

* **Adding multiline support for commit messages (closed)**
* **Some documentation fixes**
* **Enabling the passing of revision numbers instead of commit ids in histedit’s rule editor**

The first one on the list was more a hack, and it was (rightfully) closed.

Making some documentation changes/fixes is a great way of starting any interaction with the developer of any project. That was the way I started my started interacting with the mercurial devs.

* **Revision Numbers in histedit’s ruleeditor**

This was the one that has helped me personally the most. So histedit is an extrension which let’s you edit history in Mercurial. When you type hg histedit an editor pops up, where you can set the rules for the new history.

But the big drawback was that you couldn’t pass any revset. This patches cathes the Exception and fecilitates identifying the revsets.

>https://phab.mercurial-scm.org/rHG3d3cff1f6bdea90a620bd1bff87b459809bbcfb2


**Working with Mercurial, I learned a lot of things, a few of them :**

* Minimising code duplication and writing neat and efficent code adhering to certain set standards
* Eficiently debugging an issue with a debugger
* Writing tests for the codes
* Interacting with developers and communication
* OOP implemenation

At last I would like thank [Pulkit](https://www.linkedin.com/in/pulkitg25) for helping me keep progress with the timeline, and encouragement. [Jordi](https://savannah.gnu.org/users/jordigh) for his suggestion of PUDB . [Yuya](https://twitter.com/yujauja?lang=en) for reviewing my codes and pointing out my silly mistakes. And the entire Mercurial community for clearing my noob doubts when they arose.

The connection that has been with `Mercurial`, is more than just a summer of code. I will always be active in the channel, fixing up some bugs, and helping new comers to `Mercurial`.

Cheers! :beer: