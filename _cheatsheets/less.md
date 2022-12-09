---
layout: page
title: less
description: some useful ways to use less.
img: 
importance: 1
category: linux
---

## General Purposes

~~~ shell
# 
# allows to see entire file and navigate it.
less /var/log/apache2/access.log

# 
# allows to see entire file and navigate it, showing long lines without wrapping.
less -N /var/log/apache2/access.log

#
#
less -S /var/log/apache2/access.log
~~~

## Useful combinations

~~~ shell
#
# view the output of the netstat command without 
# line wrapping.
netstat -antp | less -S
~~~
