+++
draft = false
date = 2019-02-03
title = "Understanding Context Managers in Python"
slug = "ctx-in-python"
tags = ["python"]
categories = ["Tech"]
+++

So I was working on the evolve command next (`hg next`).
Here my work was with context managers,
although I had used context managers before this time I wanted to know a bit more about them.

So I read this blog on Context Managers : https://jeffknupp.com/blog/2016/03/07/python-with-context-managers/

# Few takeways:

- **Used to manage resources**
- **One of the most famous use : Handling file descriptors**

> How does a file descriptor leak ?

> By not closing the opened files. There is (usually) a limit to the number of file descriptors a process can be assigned. On UNIX-like systems, `$ ulimit -n` should give you the value of that upper limit. (It's 1024 on my PC)

- **The general syntax :**

```python
with something_that_returns_a_context_manager() as my_resource:
    do_something(my_resource)
    ...
    print('done using my_resource')
```

Now what returns a context manager ?

- Lock objects in threading
- subprocess.Popen
- pathlib.Path

### Basically any object that needs to have close called on it after use is (or should be) a context manager.

## How to create a context manager?

### The Basic Way

Define a class that contains two special methods: `__enter__()` and `__exit__()`.
`__enter__()` returns the resource to be managed (like a file object in the case of `open()` ). `__exit__()` does the cleanup work and doesn't return anything.

Let's define an `Open()` context manager using this approach:

```python
class OpenFile():
    def __init__(self,file_name, mode):
        self.file_name = file_name
        self.mode = mode
    def __enter(self):
        self.opened_file = open(self.file_name, self.mode)
        return self.opened_file
    def __exit(self):
        self.opened_file.close()
```

### The contextlib way

Since context managers are so useful there is a Standard Module dedicated to them : **`contextlib`**

There is a nice shortcut to define context managers using the `@contextmanager` decorator.
To use `@contextmaneger` to define context manager:

- Decorate a generator that yields exactly once.
- Everything before the call to yield is considered the code for `__enter__()`.
- Everything after is the code for `__exit__()`.

Let's rewrite the above **`OpenFile`** class using `@contextmanager` decorator.

```python
@contextmanager
def OpenFile(filename, mode):
    file_ = open(filename, mode)
    yield file_
    file_.close()
```

Also you can use `ContextDecorator` to create decorators that in turn will be used to create context managers. Phew!!
