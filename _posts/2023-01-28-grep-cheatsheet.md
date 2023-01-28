---
layout: post
title:  grep - cheatsheet
date:   2023-01-28 01:00:00
description: some useful ways to use grep.
language: en-us
tags: ["cheatsheet", "tools", "unix"]
language: en-us
---
## General Purposes

~~~ shell
#
# lines that beging with To:, without consider case-sensitive.
egrep -i '^To:' email.eml

# 
# filter for "failed" or "error"
# -E specifies that the pattern provided as an argument is a regular expressio
grep -E "failed|error" /var/log/auth.log

# 
# print lines that contain a sequence of three digits followed by a dash, followed by another three digits and a dash, followed by four digits 
# it is a basic regular expression
grep "\d{3}-\d{3}-\d{4}" /var/log/auth.log

# 
# a specific IPv4
grep -i "192.168.1.1" iplist.txt

#
# any IPv4 address
grep -E "\b(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\b" iplist.txt

# 
# a specific IPv6
grep -E "\b2001:0db8:85a3:0000:0000:8a2e:0370:7334\b" /etc/network/interfaces

# 
# any IPv6 address
grep -E "\b([0-9a-fA-F]{1,4}:){7}[0-9a-fA-F]{1,4}\b" /etc/network/interfaces
~~~
