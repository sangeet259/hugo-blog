+++
draft = false
date = 2019-02-05
title = "Key in Min/Max function in Python"
slug = "key-in-min_max"
tags = ["python"]
categories = [ "TIL"]
+++

So I was browsing TensorFlow doc on the Dataset API, when I stumbled across something which eventually led me to learn this:

You can pass a key function(or a lambda) to the min/max function in Python.

Here is a sweet example:
```python
def sumDigit(num):
    sum = 0
    while(num):
        sum += num % 10
        num = int(num / 10)
    return sum

# using min(arg1, arg2, *args, key)
print('Minimum is:', min(100, 321, 267, 59, 40, key=sumDigit))

# using min(iterable, key)
num = [15, 300, 2700, 821, 52, 10, 6]
print('Minimum is:', min(num, key=sumDigit))
```
Source : `https://www.programiz.com/python-programming/methods/built-in/min`