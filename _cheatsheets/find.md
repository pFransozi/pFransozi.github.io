---
layout: page
title: find
description: some useful ways to use find.
img: 
importance: 1
category: linux
---

## General Purposes

~~~ shell
# 
# searches for files with the .txt extension in the root directory 
# (/) and all of its subdirectories.
find / -name '*.txt'

# 
# searches for directories (-type d) in the root directory (/) that have names 
# that begin with "temp" (-name 'temp*').
find / -type d -name 'temp*'

# 
# searches for files with the .sh extension in the /usr/local directory 
# that are executable by the owner (-perm /u=x).
find /usr/local -name '*.sh' -perm /u=x

# 
# searches for files in the root directory (/) that have names that begin 
# with a digit ([0-9]) and end with the .txt extension. 
find / -name '[0-9]*.txt'

# 
# searches for regular files (-type f) in the /home/user directory 
# that were modified (-mtime) within the last 7 days.
find /home/user -type f -mtime -7

# 
# searches for files that are owned by the root user (-user root) 
# and have the specified permission settings (-perm /u=rwx,g=rx)
find / -user root -perm /u=rwx,g=rx.

# 
# searches for files in the current directory (.) 
# that are larger than 100 MB in size (-size +100M).
find . -size +100M
~~~

## Specific Purposes

### for privilege escalation

An attacker who is able to gain access to a system with limited privileges could use these command to:

* search for files that are owned by the root user and have weak permissions, and then try to access or modify these files to gain additional privileges on the system;
* search for world-writable directories, which can be accessed and modified by any user, and then try to create or modify files in these directories to gain additional privileges on the system;
* search for files that are owned by a group that they are a member of and have weak permissions, and then try to access or modify these files to gain additional privileges on the system;

~~~ shell
# 
# searches for files that are owned by the root user (-user root) and have the specified permission settings (-perm /u=rwx,g=rx).
find / -user root -perm /u=rwx,g=rx

# 
# searches for directories (-type d) that have the specified permission settings (-perm /u=rwx,g=rx). 
find / -type d -perm /u=rwx,g=rx

# 
# searches for files that are owned by a group (-group staff) that the attacker is a member of and have the specified permission settings (-perm /g=rwx,o=rx).
find / -group staff -perm /g=rwx,o=rx
~~~

---

An attacker who is able to gain access to a system with limited privileges can use the find command to search for programs that have these permission bits set ( SUID or SGID permission bits set), and then try to run these programs to escalate their privileges on the system.

~~~ shell

# 
# files that have the SUID permission bit set (-perm /u=s).
find / -perm /u=s

# 
# files that have the SGID permission bit set (-perm /g=s)
find / -perm /g=s

# 
# files that are owned by the root user (-user root) 
# and have the SUID permission bit set (-perm /u=s).
find / -user root -perm /u=s
~~~
