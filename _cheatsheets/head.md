---
layout: page
title: head
description: some useful ways to use head.
img: 
importance: 1
category: linux
---

## General Purposes

~~~ shell
#
# print first ten lines from /var/log/auth.log
head -n 10 /var/log/auth.log
~~~

## Useful combinations

~~~ shell
#
# first 20 lines of output from the netstat command
netstat -antp | head -n 20
~~~
