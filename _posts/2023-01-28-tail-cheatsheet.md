---
layout: post
title:  tail - cheatsheet
date:   2023-01-28 01:00:00
description: some useful ways to use tail.
language: en-us
tags: ["tail", "tool", "linux"]
language: en-us
category: ["cheatsheet"]
---
## General Purposes

~~~ shell
# 
# print the last 10 lines of the file.
tail -n 10 /var/log/apache2/access.log

#
# print the file at time it's appended
tail -f /var/log/apache2/access.log

#
# it's possible two options combined
tail -n 15 -f /var/log/apache2/access.log
~~~

## Useful combinations

~~~ shell
#
# continuously print since the file is appended, and 
# grep searches for specific keywords or patterns that may indicate a security issue.
tail -f /var/log/auth.log | grep failed
~~~
