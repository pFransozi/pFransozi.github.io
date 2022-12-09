---
layout: page
title: fgrep
description: some useful ways to use fgrep.
img: 
importance: 1
category: linux
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
