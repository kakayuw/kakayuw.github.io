---
layout: post
title: 'Python Tricks'
subtitle: 'Some useful and interesting python usage.'
date: 2020-06-16
author: Hang Yu
categories: Python
cover: '/assets/img/python.jpg'
tags: Python
---
### About Basic Types

#### Type Conversion
1. Converting `int` to `bool`.

A basic usage of `if` expression is `1 if True else 0`, however we could use builtin `int()`  function to convert a boolean value to integer. `int(True)` would return `1` and `int(False)` would return `0`. This expression could shorten the lenght of code and be more intuitive. For example, we could see `int(str1 == str2)` in counting identical strings.

