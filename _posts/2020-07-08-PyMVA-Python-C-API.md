---
toc: false
layout: post
description: Understanding Python bindings and PyMVA in ROOT
categories: [C, Python, TMVA, Interface]
title: Python C API & PyMVA
image: https://www.python.org/static/img/python-logo@2x.png
search_exclude: true
comments: true
---

Using python’s easy interface is fun!

<img alt="Can't See? Something went wrong!" src="{{site.baseurl}}/images/python_flying.jpg">


It is as much fun developing the interface for that too! Or is it?

It’s often not. It’s not easy to write efficient and robust modules in python. It’s easy to see why. While a lot of modules can be implemented in pure python, they often lag in performance. If all you care about is performance from your code, one option is to simply write code in a low level language like C++ and use it in other C++ modules. But, it’s really painful to go back to doing everything in C++ when your cool machine learning friends are using magical python libraries (I mean PyTorch :p). After using python for a while, you know how simple, powerful, and flexible it is, meeting all your Machine Learning requirements. There is little reason to go back.

For High Energy Physics, the go-to framework for big data analysis has been CERN’s ROOT framework. ROOT is a massive C++ library that even predates the STL in some areas. ROOT has a sub-module, TMVA, which houses the machine learning framework specifically developed for HEP data.  

But when you are a Python developer with a C or C++ library, you’d like to use that from Python too!

<hr/>


## Python Bindings

Python bindings allow you to call functions and pass data from Python to C or C++, letting you take advantage of the strengths of both languages. Almost all the scientific packages that you may have used in Python are developed in C++. A few examples are Numpy, Pandas, PyTorch. Numpy, for example, achieves a lot of its performance by carefully managing (and reusing) memory, and calling it from Python avoids the garbage collection overheads of writing the same functions in Python.

There are many slick tools and solutions for developing the python bindings, and if you are interested, I'd recommend reading [this](https://iscinumpy.gitlab.io/post/tools-to-bind-to-python/) awesome blog by [Henry Schreiner](https://iscinumpy.gitlab.io/page/about/) explaining various binding tools in detail. 

<hr/>

The Python API is incorporated in a C source file by including the header "Python.h".

```C++
#include <Python.h>
```

This will import everything needed to extend Python with C. Everything in python is an object and is represented as `PyObject` here. That includes everything from `int`, `str`, `list`, `dict` and `None` etc.

All user-visible symbols defined by Python.h have a prefix of `Py` or `PY`, except those defined in standard header files.

```C++
Py_Initialize();
```
This initializes the python interpreter and is required for everything we do with our TMVA PyTorch Interface.

If and when a function fails, it should set an exception condition and return an error value (usually a `NULL` pointer). Exceptions are stored in a static global variable inside the interpreter; if this variable is `NULL` no exception has occurred.

Anyways in PyMVA we are intrigued with not only making C functions callable from Python, but, the inversion is additionally utilizable. 

## Calling Python functions from C

This is especially the case for the TMVA PyTorch Interface that requires PyTorch based functions from user. Other uses are also imaginable.

Calling a Python function is relatively easy. First, the Python program must somehow pass you the Python function object. You should provide a function (or some other interface) to do this. It is generally a good practice to check the function object by `PyCallable_Check()`. Finally, when the function is called, save a pointer to the Python function object in a global variable — or wherever you see fit.

Later, when it is time to call the function, you call either of the following C functions depending on the use:

* [`PyObject_CallObject()`](https://docs.python.org/3/c-api/object.html#c.PyObject_CallObject) : Call a callable Python object *callable*, with arguments given by the tuple args. If no arguments are needed, then args can be `NULL`.

* [`PyObject_CallFunction()`](https://docs.python.org/3/c-api/object.html#c.PyObject_CallFunction) : Call a callable Python object *callable*, with a variable number of C arguments. The C arguments are described using a [`Py_BuildValue()`](https://docs.python.org/3/c-api/arg.html#c.Py_BuildValue) style format string. The format can be `NULL`, indicating that no arguments are provided.

* [`PyObject_CallMethod()`](https://docs.python.org/3/c-api/object.html#c.PyObject_CallMethod) : Call the method named *name* of object obj with a variable number of C arguments. The C arguments are described by a [`Py_BuildValue()`](https://docs.python.org/3/c-api/arg.html#c.Py_BuildValue) format string that should produce a tuple.

* [`PyObject_CallFunctionObjArgs()`](https://docs.python.org/3/c-api/object.html#c.PyObject_CallFunctionObjArgs) : Call a callable Python object *callable*, with a variable number of `PyObject*` arguments. The arguments are provided as a variable number of parameters followed by `NULL`.

* [`PyObject_CallMethodObjArgs()`](https://docs.python.org/3/c-api/object.html#c.PyObject_CallMethodObjArgs) : Calls a method of the Python object *obj*, where the name of the method is given as a Python string object in name. It is called with a variable number of `PyObject*` arguments. The arguments are provided as a variable number of parameters followed by `NULL`.

These functions on execution return a Python object pointer, this is the return value of the Python function. 

<hr/>

## ROOT PyMVA

<img alt="Can't See? Something went wrong!" src="{{site.baseurl}}/images/PyMVA_Data_Flow.png" align="right" width="60%"/>

PyMVA is a set of plugins for TMVA package based on Python that consists a set of classes that engage TMVA and allows new methods of classification and regression using external frameworks and modules.


`PyMethodBase` is the virtual base class for all PyMVA methods. It handles initialization of python interpreter, serialization, deserialization and has utilities for executing Python code in TMVA leveraging the C API for python discussed above.
`PyMethodBase` is the backbone for all PyMVA methods. Our PyTorch Interface in TMVA (under development) is built on top of **PyMVA**, `PyMethodBase` particularly!
