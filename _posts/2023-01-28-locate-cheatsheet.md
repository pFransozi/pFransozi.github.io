---
layout: post
title:  locate - cheatsheet
date:   2023-01-28 01:00:00
description: some useful ways to use locate.
language: en-us
tags: ["cheatsheet", "tools", "unix"]
language: en-us
---
## General Purpose

~~~ shell
# 
# 
locate log.txt
locate log.*
locate *log.txt

# 
# match the term exactly.
locate -b myfile.txt

# 
# contain your search term anywhere in their name
locate -i example

#
# files or directories that have 8 characters in their names.
locate -r '^.{8}$'

# 
# letters abc, which are followed by 3 digits.
locate -r 'abc[0-9]{3}'

#
# specific content, in this case .txtx
locate -r '\.txt$'
~~~
