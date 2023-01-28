---
layout: post
title:  less - cheatsheet
date:   2023-01-28 01:00:00
description: some useful ways to use less.
language: en-us
tags: ["cheatsheet", "tools", "unix"]
language: en-us
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
