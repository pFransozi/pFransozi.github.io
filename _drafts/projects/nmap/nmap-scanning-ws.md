---
layout: page
title: scanning web servers
description: understanding some nmap powerful scripts to scan web servers.
img: 
importance: 0
category: nmap
---
## Introduction

One of the most powerful features of nmap is the Nmap Scripting Engine (NSE), which is an engine that allows to execute scripts, that are developed using LUA programming language), aiming to automate a wide variety of networking tasks.

A group of those scripts is dedicated to HTTP and they are used to find vulnerabilities and misconfiguration settings in web applications. This page, which is part of Nmap Project, focus on those scripts and techiniques that are related to audit web servers and web penetration tests.

To list scripts related to HTTP, use:

~~~ shell
ls -al /usr/share/nmap/scripts/
~~~

They are text-based, so you can open using a text editor.

## Scanning Web Services

The next topics are based on the existince of a running web server, by default it can be running in 80, 443 ports. Nmap can verify that in many ways, such as next:

~~~ shell
nmap -sS -sV -O -p80,443 scanme.nmap.org 
~~~

A brief analysis of arguments:

* **-sS** is TCP/SYN scan that never completes TCP connection, which means a more unobtrusive, stealthy, and quickly way to scan. This mode requires previlege;
* **-sV** performs a service and version detection. '-sV' already executes a scan port, but if you desire a TCP/SYN scan like -sS, run as privelege user, otherwise nmap runs unprivilege mode, like -sT,which means a complete TCP connection in a way opposed to '-sS', more obstrusive, detectable, and slow;
* **-O** is OS detection and it requires privilege mode as well;
* **-p80,443** specifies ports that are known to run http services;
* **scanme.nmap.org** is used for tests, but when you have a target, you must change it to your target;
* **-v**, **-vv**, **-vvv**, **-d** are modes of verbosity and debug.

### Listing supported HTTP methods

[http-methods](https://nmap.org/nsedoc/scripts/http-methods.html){:target="_blank"} is a kind of enumeration script, which is used to list all HTTP methods available in an HTTP server. The script code can be found [here](https://github.com/nmap/nmap/blob/master/scripts/http-methods.nse).


The next command scan ports 80, 443, which are by default related to HTTP and HTTPS, testing all http methods, which are GET, HEAD, POST, OPTIONS, DELETE, PUT, CONNECT, TRACE, for HTTP; and GET, HEAD, POST, OPTIONS, TRACE, CONNECT, for HTTPS.


~~~ shell
nmap -Pn -n -p80,443 --script http-methods --script-args http-methods.test-all=true insecure.org -d
~~~

Enumeration of HTTP methods:
nmap -p80,443 --script http-methods --script-args http-methods.test-all=true scanme-nmap.org

<!-- http-methods.retest
If defined, do a request using each method individually and show the response code. Use of this argument can make this script unsafe; for example DELETE / is possible. All methods received through options are tested with generic requests. Saved status lines are shown for rest. -->

nmap -sV --script http-methods --script-args http-methods.retest scanme.nmap.org

<!-- Interesting HTTP methods
The TRACE , CONNECT , PUT , and DELETE HTTP methods might present a security risk,
and they need to be tested if supported by a web server or application.
TRACE makes applications susceptible to Cross-Site Tracing (XST) attacks and could lead
to attackers accessing cookies marked as httpOnly . The CONNECT method might allow
the web server to be used as an unauthorized web proxy. The PUT and DELETE methods
can change the contents of a folder, and this could be abused if the permissions are not
set correctly.
You can learn more about common risks associated with each method at https://
www.owasp.org/index.php/Testing_for_HTTP_Methods_and_XST_
(OWASP-CM-008 ). -->


### Discovering interesting files and folders on web servers

<https://github.com/nmap/nmap/blob/master/scripts/http-enum.nse>
<https://nmap.org/nsedoc/scripts/http-enum.html>

$ nmap --script http-enum -sV scanme.nmap.org

Using a Nikto database
Chris Sullo, the coauthor of Nikto, suggested an interesting feature that got implemented
in the NSE arsenal. The http-enum script now supports parsing Nikto database files.
The script dynamically transforms the entries to Lua tables and adds them to the existing
fingerprint database if they don't exist already. Use the http-enum.nikto-db-path
script argument to use a Nikto database:
$ nmap --script http-enum --script-args http-enum.nikto-db-
path=<Path to Nikto DB file> -sV <target>

### Brute forcing HTTP authentication

The http-brute NSE script can
perform robust dictionary attacks against HTTP basic, digest, and NTLM authentication.

$ nmap -p80 --script http-brute scanme.nmap.org

The http-form-brute script depends on the correct detection of the form fields.
Often, you will be required to manually set the name of the fields holding the username
and password variables via script arguments. If the http-form-brute.passvar script
argument is set, form detection will not be performed:

$ nmap -sV --script http-form-brute --script-args http-form-
brute.passvar=contrasenia,http-form-brute.uservar=usuario scanme-nmap.org

In a similar way, often you will need to set the http-form- brute.onsuccess
or http-form-brute.onfailure script arguments to set the success/error messages
returned when attempting to authenticate into web applications:
$nmap -sV --script http-form-brute --script-args http-form-brute.onsuccess=Exito scanme-nmap.org

Brute forcing WordPress installations
If you are targeting a popular application, remember to check whether there are any
NSE scripts specialized on attacking them. For example, WordPress installations can be
audited with the http-wordpress-brute script:
$ nmap -sV --script http-wordpress-brute scanme-nmap.org
To set the number of threads, use the http-wordpress-brute.threads script
argument:
$ nmap -sV --script http-wordpress-brute --script-args http-
wordpress-brute.threads=5 scanme-nmap.org
If the server has virtual hosting, set the host field using the http-wordpress-
brute.hostname argument:
$ nmap -sV --script http-wordpress-brute --script-args http-
wordpress-brute.hostname="jolly.coffee" scanme-nmap.org
To set a different login URI, use the http-wordpress-brute.uri argument:
$ nmap -sV --script http-wordpress-brute --script-args http-
wordpress- brute.uri="/hidden-wp-login.php" scanme-nmap.org
To change the name of the POST variable that stores the usernames and passwords,
set the http-wordpress-brute.uservar and http-wordpress-brute.
passvar arguments:
$ nmap -sV --script http-wordpress-brute --script-args
http-wordpress-brute.uservar=usuario,http-wordpress-brute.
passvar=pasguord scanme-nmap.org
Brute forcing Joomla installations
Another good example of a specialized NSE brute-force script is http-joomla-
brute . This script is designed to perform brute-force password auditing against Joomla
installations. By default, our generic brute-force script for HTTP will fail against Joomla
CMS since the application dynamically generates a security token. Still, this NSE script
will automatically fetch it and include it in the login requests. Use the following Nmap
command to launch the script:
$ nmap -sV --script http-joomla-brute scanme-nmap.org114
Scanning Web Servers
To set the number of threads, use the http-joomla-brute.threads
script argument:
$ nmap -p80 --script http-joomla-brute --script-args http-
joomla- brute.threads=5 scanme-nmap.org
To change the name of the POST variable that stores the login information, set the http-
joomla-brute.uservar and http-joomla-brute.passvar arguments:
$ nmap -sV --script http-joomla-brute --script-args http-joomla- brute.uservar=usuario,http-joomla-brute.passvar=pasguord scanme-nmap.org

### Detecting web application firewalls

Web servers are often protected by packet filtering systems that drop or redirect suspected
malicious packets. Web penetration testers benefit from knowing there is a traffic filtering
system between them and the target application. If that is the case, they can try more
rare or stealthy techniques to bypass the Web Application Firewall (WAF) or Intrusion
Prevention System (IPS).

$ nmap -sV --script http-waf-detect,http-waf-fingerprint scanme.nmap.org

To detect changes in the response body, use the http-waf- detect.
detectBodyChanges argument. I recommend that you enable it when dealing with
pages with little dynamic content:
$ nmap -sV --script http-waf-detect --script-args="http-waf-
detect.detectBodyChanges" <target>

To include more noisy attack payloads, use the http-waf-detect.aggro script
argument. This mode generates more HTTP requests, but can also trigger a response in
more products:

$ nmap -p80 --script http-waf-fingerprint --script-args http-
waf- fingerprint.intensive=1 scanme-nmap.org
To set a different URI for the probes, set the http-waf-fingerprint.root and
http-waf-detect.uri arguments:
$ nmap -p80 --script http-waf-detect --script-args http-waf-
detect.uri=/webapp/,http-waf-fingerprint.root=/webapp/ scanme-nmap.org

### Detecting possible XST vulnerabilities

Cross-Site Scripting (XSS) vulnerabilities cause XST vulnerabilities in web servers where
the TRACE HTTP method is enabled. This technique is mainly used to bypass cookie
restrictions imposed by the httpOnly directive. Penetration testers can save time using
Nmap to determine whether the web server has the TRACE method quickly.

$ nmap -sV --script http-methods,http-trace --script-args
http-methods.retest <target>

$ nmap -sV --script http-methods,http-trace --script-args http-
methods.retest,http-trace.path=/secret/,http-methods.url-path=/
secret/ <target>

### Detecting XSS vulnerabilities

XSS vulnerabilities allow attackers to spoof content, steal user cookies, and even execute
malicious code on the user's browsers. Web penetration testers can use Nmap to discover
these vulnerabilities in web servers in an automated manner. Although there is room for
improvement in this module, it is a good start for XSS testing.

nmap -sV --script http-unsafe-output-escaping <target>

If you are working with a PHP server, run the following Nmap command:
$nmap -sV --script http-phpself-xss,http-unsafe-output-
escaping <target>

Important Note
The official documentation of the http-unsafe-output- escaping
and http-phpself-xss scripts can be found at the following URLs:
https://nmap.org/nsedoc/scripts/http-phpself-xss.
html
https://nmap.org/nsedoc/scripts/http-unsafe-
output-escaping.html

The http-xssed script queries the online database http://xssed.com/ , the biggest
archive of websites vulnerable to XSS vulnerabilities. Use the following command to check
whether the web server you are scanning has been previously reported:
$ nmap -sV --script http-xssed <target>

### Finding SQL injection vulnerabilities

SQL injection vulnerabilities are caused by the lack of sanitation of user input, and they
allow attackers to execute Database Management System (DBMS) queries that could
compromise the entire system. This type of web vulnerability is widespread, and because
each script variable must be tested, checking for such vulnerabilities can be a very tedious
task. Fortunately, we can use Nmap to scan a web server looking for SQL injection
vulnerabilities quickly.
This recipe shows how to find SQL injection vulnerabilities in web applications
with Nmap NSE.

$ nmap -sV --script http-sql-injection <target>

It crawls a web server looking for forms and URIs with parameters and attempts to find
SQL injection vulnerabilities. The script determines whether the server is vulnerable by
inserting SQL queries that are likely to cause an error in the application. Keep in mind this
limitation as the script will not detect any blind SQL injection vulnerabilities.
The error messages that the script matches are read from an external file located by default
at /nselib/data/http-sql-errors.lst . This file was taken from the fuzzdb
project ( https://github.com/fuzzdb-project/fuzzdb ), and users may choose
an alternate file if needed.

### Finding web applications with default credentials

Default credentials are often forgotten in web applications and devices, such as webcams,
printers, VoIP systems, video conference systems, and other appliances. There is a handy
NSE script to automate the process of testing default credentials in the network. Several
popular products are supported, including web applications, such as Apache Tomcat
Manager, Oracle Administration Console, F5 Big IP, CitrixNetScaler, Cacti, printers,
and even the web management interfaces of home routers.
This recipe shows you how to automatically test default credential access in several web
applications with Nmap.

nmap -sV --script http-default-accounts <target>

I developed this NSE script to save time during web penetration tests by automatically
checking whether system administrators have forgotten to change any default passwords
in their systems. I've included a few fingerprints for popular services, but this script can
be significantly improved by supporting more services. If you have access to a service
commonly configured with default credential access, I encourage you to submit new
fingerprints to its database. Recently, nnposter posted a big update that improves this
script and its database.
The script detects web applications by looking at known paths and initiating a login
routine using the predefined default credentials. It depends on a fingerprint file located
at /nselib/data/http-default-accounts.nse . Entries are Lua tables, and
they look like the following:

There's more...
For less intrusive scans, filter out probes by category using the http-default-
accounts.category script argument:
$ nmap -sV --script http-default-accounts --script-args http-
default-accounts.category=routers <target>
The available categories are as follows:
• Web: This category manages web applications.
• Router: This category manages the interfaces of routers.
• VoIP: This category contains VoIP devices.
• Security: This category manages security-related software.
• Industrial: This category manages software related to Industrial Control
Systems (ICSes).
• Printer: This category manages printer devices.
• Storage: This category contains storage devices.
• Virtualization: This category manages software for virtualization.
• Console: This category contains remote consoles.

This script uses the root folder as the base path by default, but you can set a different one
using the http-default-accounts.basepath argument:
$ nmap -sV --script http-default-accounts --script-args http-
default-accounts.basepath=/web/ <target>
The default fingerprint file is located at /nselib/data/http-default-accounts-
fingerprints.lua , but you can use a different file by specifying the http-
default-accounts.fingerprintfile argument:
$ nmap -sV --script http-default-accounts --script-args http-
default-accounts.fingerprintfile=./more-signatures.txt <target>

### Detecting insecure cross-domain policies