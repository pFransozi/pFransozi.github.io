---
layout: post
title:  head - cheatsheet
date:   2023-01-28 01:00:00
description: some useful ways to use head.
language: en-us
tags: ["cheatsheet", "tools", "unix"]
language: en-us
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
