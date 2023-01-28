---
layout: post
title:  nmap - cheatsheet
date:   2023-01-28 01:00:00
description: some useful ways to use nmap.
language: en-us
tags: ["nmap", "tool"]
language: en-us
category: ["cheatsheet"]
---
## My faves

~~~ shell
nmap -T4 -A -Pn -vv scanme.nmap.org
nmap -A -sS -vv scanme.nmap.org
nmap -sS -sU -vv scanme.nmap.org
nmap -sX scanme.nmap.org
~~~

## General Purpose

~~~ shell
nmap -e eth2 scanme.nmap.org  # It specifies a different network interface.

nmap scanme.nmap.org # By default, it shows DNS info, host status, 
                     # interesting ports with their states and service names. 
                     # If running **as a privileged user** 
                     # it launches a SYN stealth scan, otherwise, 
                     # it uses TCP connect scan.

nmap --dns-servers 8.8.8.8,8.8.4.4 scanme.nmap.org  # It specifies DNS servers.

nmap -n scanme.nmap.org # No DNS resolution.

nmap -PN scanme.nmap.org  # No ping option, which means it's not check 
                          # whether the target is online.

nmap 192.168.1.1/24  # It specifies IP range.
nmap scanme.nmap.org/24
nmap 192.168.1.1 192.168.1.2 192.168.1.3
nmap 192.168.1.1-3
nmap 192.168.1.*

echo 10.10.0.5 > scan.txt  # Or using a file, which can be combined 
                           # with any option or method, except --exclude and 
                           # --exclude-file options.
echo 10.10.0.6 >> scan.txt
nmap -iL scan.txt

nmap 192.168.1.1-255 --exclude 192.168.1.1  # It excludes IPs.
nmap 192.168.1.1-255 --exclude 192.168.1.1,192.168.1.2

echo 10.10.0.5 > dontscan.txt # Or using a file.
echo 10.10.0.6 >> dontscan.txt
nmap --exclude-file dontscan.txt 10.10.0.1-255
~~~

---

## Detection feature

~~~ shell
nmap -O scanme.nmap.org # OS detection. It requires raw packets, 
                        # then must be executed in **privileged mode**.

nmap -O --osscan-guess scanme.nmap.org  # If basic OS detection feature fails, 
                                        # it can be used an argument 
                                        # to force Nmap to guess the OS.

nmap -sV scanme.nmap.org # Service and version detection.

# Intensity of detection can be changed from 0 to 9. 
# The most intense is effective against services running on non-default ports.
nmap -sV --version-intensity 9 scanme.nmap.org 

# Aggressive detection mode enables: 
# OS detection, version detection, script scanning, and traceroute.
nmap -A scanme.nmap.org
~~~

## Port Scanning modes

~~~ shell
# TCP SYN scan performs a quickly, unobtrusive and stealthy 
# since it never completes TCP connection (half-open scanning).
# It **requires previlege** to access a raw packet.
nmap -sS scanme.nmap.org

# TCP connect scan is default when user does not have previlege 
# to access a raw packet, becoming a choice when SYN is not available.
# It is less efficient since Nmap has less control because it does not
# handle raw packet. Then, the OS call completes TCP connection rather
# than performing the half-open reset that SYN scan does.
# As consequence: it takes longer, it requires more packets and target machines
# are more likely to log the connection.
nmap -sT scanme.nmap.org 

# UDP scans.
nmap -sU scanme.nmap.org 

# SCTP INIT scan.
nmap -sY scanme.nmap.org 

# the next three techniques exploit loophole in the TCP RFC to differentiate 
# between open and closed ports. When scanning systems compliant with this 
# RFC 793 text, any packet not containing SYN, RST, or ACK bits will result 
# in a returned RST if the port is closed and no response at all if the port is open.
# Pros: they can sneak through certain non-stateful firewalls and packet filtering
# routers, are a little more stealthy than even a SYN scan;
# Cons: not all systems follow RFC 793 to the letter, they can't distinguish open ports 
# from certain filtered ones.
nmap -sN scanme.nmap.org # Null scan does not set any bit in TCP header flag.
nmap -sF scanme.nmap.org # FIN scan set just TCP FIN bit
nmap -sX scanme.nmap.org # Xmas scan set FIN, PSH, and URG flags.


nmap -sA scanme.nmap.org # TCP ACK scan.
nmap -sW scanme.nmap.org # TCP Window scan.
nmap -sM scanme.nmap.org # TCP Maimon scan.

# Custom TCP scan. --scanflags URGACKPSHRSTSYNFIN sets everything
nmap --scanflags scanme.nmap.org 
nmap -sZ scanme.nmap.org # (SCTP COOKIE ECHO scan)
~~~

---

## Network Scanning
