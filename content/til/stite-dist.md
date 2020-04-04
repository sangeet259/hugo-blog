+++
draft = false
date = 2019-02-22
title = "Python Site Packages vs Dist Packages"
slug = "python-site-vs-dist-packages"
tags = ["python"]
categories = [ "TIL"]
+++

There are certain locations Python looks for installed packages.
You can find out the locations :
```python
import sys
print '\n'.join(sys.path)
```
```bash
/usr/lib/python2.7
/usr/lib/python2.7/plat-x86_64-linux-gnu
/usr/lib/python2.7/lib-tk
/usr/lib/python2.7/lib-old
/usr/lib/python2.7/lib-dynload
/home/sangeet/.local/lib/python2.7/site-packages
/usr/local/lib/python2.7/dist-packages
/usr/lib/python2.7/dist-packages
```


dist-package in specific to Debian/Ubuntu distros.
The modules that comes from a Debian Package manager are installed in the dist-packages.
`/usr/lib/python2.7/dist-packages`

Since pip also comes from a Debian package manager, they too use 'dist-packages' in Debian/Ubuntu. But their path is 'a bit' different. 
`/usr/local/lib/python2.7/dist-packages`



But if you install Python from source, that Python will use site-packages instead of dist-packages.

This dist-package logic exists to separate the two installations separate, because there are many Ubuntu/Debian utilities which relies on the Python that comes pre-packaged.




References:
* https://stackoverflow.com/a/9388115/6507303
* https://leemendelowitz.github.io/blog/how-does-python-find-packages.html