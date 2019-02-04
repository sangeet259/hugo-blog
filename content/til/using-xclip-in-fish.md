+++
draft = false
date = 2019-01-11
title = "XClip in Fish Shell"
slug = "xclip-in-fish"
tags = ["devops","fish"]
categories = [ "TIL"]
+++
Wondered if you could just copy the output of a bash command to clipboard and paste it somewhere.

XClip is here for your rescue. The Fish Shell 3.0 now supports xclip.

How to use :

* For copying
  * `$pwd | xclip`
* Paste it
  * `$cd (xclip -o)`

PS: In fish to execute a string as a command use paranthesis `"( )"` instead of backticks " \` \` ".
 Just add alias in the fish config : 
```bash
alias c "xclip"
alias v "xclip -o"
```

For bash the 
```bash
alias "c=xclip"
alias "v=xclip -o"
```
