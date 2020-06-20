---
layout: post
title: 'Python Tricks'
subtitle: 'Some useful and interesting python usage.'
date: 2020-06-16
author: Hang Yu
categories: Python
# cover: '/assets/img/python.jpg'
cover: 'https://cdn.cjr.org/wp-content/uploads/2019/07/AdobeStock_100000042-e1563305717660-1300x500.jpeg'
tags: Python
---
### About Basic Types

#### Type Conversion
1. Converting `int` to `bool`.

A basic usage of `if` expression is `1 if True else 0`, however we could use builtin `int()`  function to convert a boolean value to integer. `int(True)` would return `1` and `int(False)` would return `0`. This expression could shorten the lenght of code and be more intuitive. For example, we could see `int(str1 == str2)` in counting identical strings.

2. Converting `int` to `1` or `0`

Similarly, we could just convert an integer to a boolean variable and then convert it back to int. For example, instead of `1 if x != 0 else 0` we could write `int(not x)`.

3. Converting an integer to binary string

We could use builtin `bin()` to easily convert any integer into a binary string, which starts from `0x`. If we only need the string without any prefix, `bin(string)[2:]` would be effect. There is still other things to mention, like converting a negative integer into its binary form. It would not direct give us wanted value under the 32bit or 64bit environment, it would add a neg sign, e.g. `-0xb`, which will disapoint that intend to take advantage of this trick.

