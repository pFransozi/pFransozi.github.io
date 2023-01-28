---
layout: post
title:  awk - cheatsheet
date:   2023-01-28 01:00:00
description: some useful ways to use awk.
language: en-us
tags: ["cheatsheet", "tools", "unix"]
language: en-us
---
## General Purposes

~~~ shell
# 
# search "include" and print the line.
awk '/include/ { print }' code.file
awk '/error/ {print}' mylogfile.log

# 
# to print the first and third columns of each line in a text file
awk '{print $1, $3}' example.txt

# 
# to calculate the sum of all the numbers in a column of a text file
awk '{sum+=$1} END {print sum}' myfile.txt

# 
# To search for a specific pattern in a text file and print the line 
# that contains the pattern, as well as the line before and after it
awk '/hello/{print; print x; print} {x=$0}' myfile.txt

# 
# To calculate the total number of log entries in a log file
awk 'END {print NR}' mylogfile.log

# 
# To find the average time between log entries in a log file
awk '{sum+=$1; count++} END {print sum/count}' mylogfile.log
~~~
