---
layout: post
title:  egrep - cheatsheet
date:   2023-01-28 01:00:00
description: some useful ways to use egrep.
language: en-us
tags: ["egrep", "tool", "linux"]
language: en-us
category: ["cheatsheet"]
---
## General Purposes

~~~ shell

# 
# uses [] character class to match any alphanumeric character at the beginning 
# of each line in filename.txt. This character class is not supported by grep.
egrep '^[a-zA-Z0-9]+$' filename.txt 

# 
# uses -v option to invert the search, so it matches all lines in filename.txt 
# that do not begin with #. This option is not supported by grep.
egrep -v '^#' filename.txt

# 
# uses [^] negated character class to match any line in filename.txt that begins with a character that is not #. This character class is not supported by grep.
egrep '^[^#].*' filename.txt 

# 
# -o option to only print the matched text, rather than the entire line. 
# It also uses the [[:alpha:]] character class to match any alphabetic character. 
# This character class is not supported by grep.
egrep -o '[[:alpha:]]{5,}' filename.txt

# 
# -c option to only print the number of matches found, 
# rather than the matched lines. It also uses the {n} quantifier 
# to match a specific number of digits in a phone number pattern. 
# This quantifier is not supported by grep.
egrep -c '[0-9]{3}-[0-9]{3}-[0-9]{4}' filename.txt

# 
# | alternation operator and the + quantifier to match lines in filename.txt 
# that contain one or more repetitions of the words "foo" or "bar". 
# These regular expression features are not supported by grep.
egrep '(foo|bar)+' filename.txt
~~~
