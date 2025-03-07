+++
title = "Install numpy and troubles"
date = 2024-08-19
updated = 2024-12-26
description = "Install numpy to macos by brew"

[taxonomies]
tags = ["instruction", "tutorial", "numpy", "Apple Sillicon", "MacOS", "brew", "python"]
+++

## Nothing wrong but...

Once I needed in `numpy` package on my laptop. I try to install as normal pip package by `python3 -m pip install numpy` but unfortunatelly it generates errors like this:

```zsh
% python3 -m pip install numpy
error: externally-managed-environment

× This environment is externally managed
╰─> To install Python packages system-wide, try brew install
    xyz, where xyz is the package you are trying to
    install.
    ... bla bla bla bla bla
```

Okay, said I, lets install by brew.

## Brew and issues who comes from anniversary begins

Installing by brew will be without surprise things I. `brew install numpy` and it was enough? Yes, but after run the python and imported package I encount another one issue:

```python
>>> import numpy as np
>>> np.matrix([[1,2],[3,4]])
Traceback (most recent call last):
  File "<python-input-1>", line 1, in <module>
    np.matrix([[1,2],[3,4]])
    ^^^^^^^^^
AttributeError: module 'numpy' has no attribute 'matrix'
>>> 
>>> dir(np)
['__doc__', '__file__', '__loader__', '__name__', '__package__', '__path__', '__spec__']
>>> np.matrix
Traceback (most recent call last):
  File "<python-input-2>", line 1, in <module>
    np.matrix
AttributeError: module 'numpy' has no attribute 'matrix'
>>> np.array
Traceback (most recent call last):
  File "<python-input-3>", line 1, in <module>
    np.array
AttributeError: module 'numpy' has no attribute 'array'
>>> np.dot
Traceback (most recent call last):
  File "<python-input-4>", line 1, in <module>
    np.dot
AttributeError: module 'numpy' has no attribute 'dot'
>>> np.__file__
>>> np.__name__
'numpy'
>>> np.__package__
'numpy'
>>> np.__path__
_NamespacePath(['/opt/homebrew/lib/python3.13/site-packages/numpy'])
```

**After going to the install path I saw that each directory was empty!!!** Crazy day. After googling I found some [closed issue](https://github.com/Homebrew/homebrew-core/issues/15698) in brew github.

The final soultion was: `brew link --overwrite numpy`. Why since 2017 nobody haven't fixed this part of broken code?!

## Conclusions

If you're installing something from system maintained package be sure that your installation is correct. Use overrides, like `virtual env` instead of brew.
