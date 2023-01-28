---
layout: post
title:  fgrep - cheatsheet
date:   2023-01-28 01:00:00
description: some useful ways to use fgrep.
language: en-us
tags: ["cheatsheet", "tools", "unix"]
language: en-us
---
## General Purposes

~~~ shell
# 
# searches for the string "hello" in filename.txt.
fgrep 'hello' filename.txt

#
# searches for the string "hello" in filename.txt, ignoring the case of the letters.
fgrep -i 'hello' filename.txt

# 
# searches for lines in filename.txt that do not contain the string "hello".
fgrep -v 'hello' filename.txt

#
#counts the number of lines in filename.txt that contain the string "hello".
fgrep -c 'hello' filename.txt
~~~
