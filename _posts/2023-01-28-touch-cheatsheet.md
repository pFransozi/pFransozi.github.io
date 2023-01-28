---
layout: post
title:  touch - cheatsheet
date:   2023-01-28 01:00:00
description: some useful ways to use touch.
language: en-us
tags: ["touch", "tool", "linux"]
language: en-us
category: ["cheatsheet"]
---
## General Purpose

~~~ shell

touch main.py
touch list_1.txt list_2.txt

# 
# with specific date time (YYMMDDHHMM.SS).
touch -t 2101011200.00 myfile.txt

# 
# To create a file with specific permissions, 
# you can use the -m and -a options followed by the permissions in octal format.
touch -m -a=0644 myfile.txt

#
# can be used to create new, empty files that can be used 
# to replace suspicious or malicious files
touch malware.exe

#
# to create new files with specific timestamps or permissions
touch -t 2101011200.00 -m -a=0644 myfile.txt
~~~
