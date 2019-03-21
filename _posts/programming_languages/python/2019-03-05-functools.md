---
title: python functools library
category: python
comments: true
excerpt_separator: <!--more-->
layout: post
tags: [functools, partial]
---
Experience towards how to use functools library of python 3.xx

Topics include: partial
<!--more-->

[Libraty Path](https://docs.python.org/3/library/functools.html)

## partial
### Official introduction
Return a new partial object which when called will behave like func called with the positional arguments args and keyword arguments keywords. If more arguments are supplied to the call, they are appended to args. If additional keyword arguments are supplied, they extend and override keywords. Roughly equivalent to:
```python
def partial(func, *args, **keywords):
    def newfunc(*fargs, **fkeywords):
        newkeywords = keywords.copy()
        newkeywords.update(fkeywords)
        return func(*args, *fargs, **newkeywords)
    newfunc.func = func
    newfunc.args = args
    newfunc.keywords = keywords
    return newfunc
```
### Usage example:
```python
import functools

def add(a, b):
    print('%d+%d=%d' % (a, b, a+ b))

add_3 = functools.partial(add, 3)
add_4 = functools.partial(add, b=4)

add(3, 5) # Output: 3+5=8
add_3(5) # Output: 3+5=8
add_4(5) # Output: 5+4=9
```
In the example about, we fist define a function called "add", which print the result of adding two numbers. Then we define a function with "partial" called "add_3" which always set the first parameter "a" to 3 and accept only one parameter "b". And we also define another function called "add_4" which always set the second parameter "b" to 4 and accept only one parameter "a".

Thus, when we call add_3(5), it equals to call add(3, 5). When we call add_4(5), it equals to call add(5, 4).

This function is very useful if you want inject some parameters into a function. For example, you trying to manage a web session, you can get the userId from database and inject this parameter into following request processing procedure.
