---
layout: post
title:  scanning web servers
date:   2023-01-28 01:00:00
description: understanding some nmap powerful scripts to scan web servers
language: en-us
tags: ["web-servers"]
language: en-us
category: ["nmap", "reconnaissance-tool"]
---
## Introduction

One of the most powerful features of nmap is the nmap scripting engine (nse), which is an engine that allows to execute scripts that are developed using lua programming language, aiming to automate a wide variety of networking tasks.

A group of those scripts is dedicated to http and they are used to find vulnerabilities and misconfiguration settings in web applications. This page, which is part of nmap project, focus on those scripts and techniques that are related to audit web servers and web penetration tests.

To list scripts related to http, use: `ls -al /usr/share/nmap/scripts/ | grep http`. They are text-based, so you can open using a text editor.

## Scanning Web Services

The next topics are based on the existence of a running web server, by default it can be running in 80, 443 ports, in public network. Nmap can verify if there is a web service running on those ports in many ways. One example is: `nmap -sS -sV -O -p80,443 scanme.nmap.org`. Considering public services, it's common that they are running on their default ports. Otherwise, it may be required a deeper scanning to detect which port the service runs on, if any, such as `nmap -sS -sV -O -p1024-20000 -T4 scanme.nmap.org`.

A brief analysis of those flags:

* `-sS` is tcp/syn scan that never completes tcp connection, which means a more unobtrusive, stealthy, and quickly way to scan. This mode requires privilege;
* `-sV` performs a service and version detection, as well as a service scan. If in a privilege mode will be a `-sS` flag, tcp/syn, otherwise in an unprivileged mode, service scan will be `-sT`, which means a complete tcp connection is a way opposed to `-sS`, more obtrusive, detectable, and slow;
* `-O` is os detection and it requires privilege mode as well;
* `-p80,443` specifies ports that are known to run http services;
* `-p1025-20000` specifies ports that are out of the default range. For a more aggressive approach it can be used `-p-` which will scan all 65535 ports;
* `-T4` specifies a timing template known as aggressive, but it can be adjusted from `-T0` to `-T5` depends on the network from the command is sent and the target network.
* `scanme.nmap.org` is used for tests, but when you have a target, you must change it to your target;

That command returns something like this:

~~~ log
Starting Nmap 7.93 ( https://nmap.org ) at 2022-12-23 23:34 UTC
Nmap scan report for scanme.nmap.org (45.33.32.156)
Host is up (0.26s latency).
Other addresses for scanme.nmap.org (not scanned): 2600:3c01::f03c:91ff:fe18:bb2f

PORT    STATE    SERVICE VERSION
80/tcp  filtered http
443/tcp closed   https
Too many fingerprints match this host to give specific OS details

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 11.89 seconds
~~~

No level of verbosity gives a bottom line result. Otherwise, there are several verbosity levels available in nmap, which are specified using `-v`, `-d`, and `-V` options followed by a number or a combination of the letters. The higher the verbosity level, the more detailed the output will be. Here is a breakdown of the available verbosity levels:

* `-v` is the default verbosity level, and it provides normal output with a summary at the end of the scan;
* `-vv` or `-v2` provides more detailed output, including the reason why each port was marked as open or closed;
* `-vvv` or `-v3` provides even more detailed output, including all of the packets sent and received during the scan;
* `-V` provides less output than the default, but includes version and os detection results;
* `-d` increases debugging output;
* `-dd` or `-d2` increases debugging output even further;
* `-ddd` or `-d3` increases debugging output to the maximum level;

A multiple verbosity options can be used together, such as `-v` and `-d` to increase the level of detail in the output.

### Listing supported http methods

[http-methods](https://nmap.org/nsedoc/scripts/http-methods.html){:target="_blank"} is a kind of enumeration script, which is used to list all http methods available in an http server. The script code can be found [here](https://github.com/nmap/nmap/blob/master/scripts/http-methods.nse).

The next command scans ports 80, 443, which are by default related to http and https, testing all http methods, which are get, head, post, options, delete, put, connect, trace, for http; and get, head, post, options, trace, connect, for https: `nmap -p80,443 --script http-methods --script-args http-methods.test-all=true insecure.org -d`.

There are two main flags when using nmap and nse, which are `--script` and `--script-args`. The latest is used when the script allow to send parameters to it, as example: `--script-args='http-methods.test-all=true,http-methods.url-path=/mypath/'` In the case of the http-methods script, main parameters are:

* `http-methods.url-path` is the path to request and by default is `/`, or change it to `http-methods.url-path=/mypath/`;
* `http-methods.test-all` is a boolean field that test all methods even the unsafe ones;
* `http.useragent` changes the value of user-agent, which is by default `Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org/book/nse.html)`. To suppress that http field passes a empty string;
* `http-methods.retest`is used to test each method individually;

By default, http-method script uses http options method to ask to the web server which are the http methods available for a specific resource, by default `/` root web path. When `http-methods.retest` parameter is used, nmap forge a request for each method and it analyzes the web server return. The options method is implemented in web servers to inform clients of its supported methods, but this method does not consider configuration or firewall rules, and having an http method listed by options does not necessarily mean that it is accessible to you.

For example, without http-methods.retest the request is:

~~~ http
OPTIONS /resource HTTP/1.1
Host: www.example.com
User-Agent: MyClient/1.0
~~~

And a possible response is:

~~~ http
Allow: GET, HEAD, POST, PUT, DELETE, TRACE, OPTIONS
Content-Length: 0
Date: Mon, 21 May 2022 10:00:00 GMT
~~~

With http-methods.retest, nmap generates a http request for each method and it will analyze each response status code, as follow:

* **GET request**:

~~~ http
GET /resource HTTP/1.1
Host: www.example.com
User-Agent: MyClient/1.0
~~~

* **GET response**:

~~~ http
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 1234
Date: Mon, 21 May 2022 10:00:00 GMT

<html>
  <body>
    <p>This is the content of the resource.</p>
  </body>
</html>
~~~

* **HEAD request**:

~~~ http
HEAD /resource HTTP/1.1
Host: www.example.com
User-Agent: MyClient/1.0
~~~

* **HEAD response**:

~~~ http
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 1234
Date: Mon, 21 May 2022 10:00:00 GMT
~~~

* **POST request**:

~~~ http
POST /resource HTTP/1.1
Host: www.example.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 25
User-Agent: MyClient/1.0

key1=value1&key2=value2
~~~

* **POST response**:

~~~ http
HTTP/1.1 201 Created
Location: /resource/123
Content-Type: text/html
Content-Length: 0
Date: Mon, 21 May 2022 10:00:00 GMT
~~~

* **PUT request**:

~~~ http
PUT /resource/123 HTTP/1.1
Host: www.example.com
Content-Type: application/json
Content-Length: 45
User-Agent: MyClient/1.0

{"key1": "value1", "key2": "value2"}
~~~

* **PUT response**: 

~~~ http
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 0
Date: Mon, 21 May 2022 10:00:00 GMT
~~~

* **DELETE request**:

~~~ http
DELETE /resource/123 HTTP/1.1
Host: www.example.com
User-Agent: MyClient/1.0
~~~

* **DELETE response**:

~~~ http
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 0
Date: Mon, 21 May 2022 10:00:00 GMT
~~~

* **TRACE request**:

~~~ http
TRACE /resource HTTP/1.1
Host: www.example.com
User-Agent: MyClient/1.0
~~~

* **TRACE response**:

~~~ http
HTTP/1.1 200 OK
Content-Type: message/http
Content-Length: 123
Date: Mon, 21 May 2022 10:00:00 GMT

TRACE /resource HTTP/1.1
Host: www.example.com
User-Agent: MyClient/1.0
~~~

* **CONNECT request**

~~~ http
CONNECT example.com:443 HTTP/1.1
Host: example.com
User-Agent: MyClient/1.0
~~~

* **CONNECT response**

~~~ http
HTTP/1.1 200 Connection established
~~~

Return to the command above, let's analyze its return. It starts with a ping scan, which is used to discover if the host is online, and dns resolution:

~~~ log
Initiating Ping Scan at 00:51
Scanning insecure.org (45.33.49.119) [2 ports]
Completed Ping Scan at 00:51, 0.31s elapsed (1 total hosts)

mass_rdns: Using DNS server 181.213.132.2
mass_rdns: Using DNS server 181.213.132.3
Initiating Parallel DNS resolution of 1 host. at 00:51
mass_rdns: 0.58s 0/1 [#: 2, OK: 0, NX: 0, DR: 0, SF: 0, TR: 1]
Completed Parallel DNS resolution of 1 host. at 00:51, 0.58s elapsed
DNS resolution of 1 IPs took 0.58s. Mode: Async [#: 2, OK: 1, NX: 0, DR: 0, SF: 0, TR: 1, CN: 0]
~~~

To skip ping scan `-Pn` flag can be used, and `-n` to skip dns resolution. Then, specified ports are tested and scripts are executed to discover all http methods:

~~~ log
Initiating Connect Scan at 00:51
Scanning insecure.org (45.33.49.119) [2 ports]
Discovered open port 443/tcp on 45.33.49.119
Discovered open port 80/tcp on 45.33.49.119
Completed Connect Scan at 00:51, 0.24s elapsed (2 total ports)

NSE: Script scanning 45.33.49.119.
NSE: Starting runlevel 1 (of 1) scan.
Initiating NSE at 00:51
NSE: Starting http-methods against insecure.org (45.33.49.119:80).
NSE: Starting http-methods against insecure.org (45.33.49.119:443).
NSE: [http-methods 45.33.49.119:80] HTTP Status for OPTIONS is 301
NSE: [http-methods 45.33.49.119:80] No Allow or Public header in OPTIONS response (status code 301)
NSE: [http-methods 45.33.49.119:443] HTTP Status for OPTIONS is 200
NSE: [http-methods 45.33.49.119:80] Response Code to Random Method is 301
NSE: [http-methods 45.33.49.119:443] Response Code to Random Method is 501
NSE: Finished http-methods against insecure.org (45.33.49.119:443).
NSE: Finished http-methods against insecure.org (45.33.49.119:80).
Completed NSE at 00:51, 6.14s elapsed
~~~

The results will include the supported methods for every web server detected on ports 80 or 443, as follow:

~~~ log
PORT    STATE SERVICE REASON
80/tcp  open  http    syn-ack
| http-methods: 
|   Supported Methods: GET HEAD POST OPTIONS DELETE PUT CONNECT TRACE
|_  Potentially risky methods: DELETE PUT CONNECT TRACE
443/tcp open  https   syn-ack
| http-methods: 
|   Supported Methods: OPTIONS GET HEAD POST TRACE CONNECT
|_  Potentially risky methods: TRACE CONNECT
~~~

Using this `http-methods.retest`, the result specifies each http method tested:

~~~ log
PORT    STATE SERVICE REASON
80/tcp  open  http    syn-ack
| http-methods: 
|   Supported Methods: GET HEAD POST OPTIONS DELETE PUT CONNECT TRACE
|   Potentially risky methods: DELETE PUT CONNECT TRACE
|   Status Lines: 
|     HEAD: HTTP/1.1 301 Moved Permanently
|     TRACE: HTTP/1.1 301 Moved Permanently
|     DELETE: HTTP/1.1 301 Moved Permanently
|     POST: HTTP/1.1 301 Moved Permanently
|     PUT: HTTP/1.1 301 Moved Permanently
|     CONNECT: HTTP/1.1 400 Bad Request
|     GET: HTTP/1.1 301 Moved Permanently
|_    OPTIONS: HTTP/1.1 301 Moved Permanently
443/tcp open  https   syn-ack
| http-methods: 
|   Supported Methods: OPTIONS GET HEAD POST TRACE CONNECT
|   Potentially risky methods: TRACE CONNECT
|   Status Lines: 
|     HEAD: HTTP/1.1 200 OK
|     OPTIONS: HTTP/1.1 200 OK
|     CONNECT: HTTP/1.1 400 Bad Request
|     TRACE: HTTP/1.1 200 OK
|     GET: HTTP/1.1 200 OK
|_    POST: HTTP/1.1 200 OK
~~~

Those methods are worthy of further discussion that goes beyond the context of this page, but let's understand why nmap considers some of them a security risk. 

> First of all, if the http-method script lists a method as potentially risk, it may not all be a risk and it must be check to make sure. 

> Second of all, http-method script considers a risk: trace, connect, put and delete.

> Third of all, just for reference, rfc 9110 considers secure HTTP methods those that do not alter the state of the server, leading to a read-only operation. Thus, the client doesn't request any server change

**Trace method** requests an application-level loop-back of the request message, back to the client from the web server as the content of a 200 (OK) response. Trace allows the client to see what is being received at the other end of the request chain. 

Historically, it could be used to bypass httponly cookie flag on cookies, vulnerability known as Cross Site Tracing, and was used to disclosure sensitive information from http headers that were restricted from javascript and cookies. **That kind of attack is no longer possible in modern web browsers**. 

Otherwise, it can lead to disclosure of sensitive information such as internal authentication headers appended by reverse proxies or web application firewalls in a way to bypass those such as [this example](https://www.blackhillsinfosec.com/three-minutes-with-the-http-trace-method/), using Burpsuite or ZAP.

> Anyway, the trace method should be disabled on production web servers even if only to comply one of the wisest security principles says that what is unused should be disabled.

**Connect method** requests a tunnel to the destination origin server by the web client and, if so, thereafter restricts its behavior to blind forwarding of data, until the tunnel is closed. Tunnels are primarily, but not exclusively, used in combination with the transport layer security (TLS) to create a secure connection between a client and a server to stablish an end-to-end encrypted session.

> Connect is intended for use in requests to a proxy server.

It's may be another historically issue, since I did not find any concrete explanation how http connection method can be exploited, except for 10-year-old references. Anyway, the main problem is, or was, that the http connect method can be abused to establish arbitrary TCP connections through vulnerable proxy services. That abuse could be: a vulnerable proxy service on one network as an intermediary to scan or connect to TCP services on another network; a connection from a public network, such as the Internet, through a vulnerable proxy service to an internal network; If a proxy service allows recursive connections, an attacker may be able to cause a denial-of-service condition by consuming resources by making repeated connections from the proxy service back to itself. But as I said, I didn't find any recent reference about this problem, which could indicate that the modern proxy web server are not more vulnerable to this.

**Put method** requests that the state of the target resource be created or replaced with the state defined by the representation enclosed in the request message context. If the target resource does not have a current representation and the put successfully creates one, then the server must inform sending a 201 response. If the target resource does have a current representation and that representation is successfully modified in accordance with the state of the enclosed representation, then the server must send either a 200 or a 204 response. There're as well as responses that reflects inconsistences with the target resource such as 409 or 415.

It's important to note that the put method is idempotent, which means that multiple identical put requests should have the same effect as a single request, and for that information is altered. Unlike post method, by which every request may have additional effects.

Overwriting resource is a possible vulnerability related to put method, sending a put request to overwriting an existent resource in the server. Mass assignment is another vulnerability by which an attacker may change properties of an object that are not initially specified. And last but not least, since put is an input method it is vulnerable to injection attack, requiring properly sanitize and validate any user input.

**Delete method** requests that a resource be removed from the server. This method is idempotent and not reversible. Likewise put method and others input methods, delete is vulnerable to method misuse, since delete is not implemented properly and user can delete resources that they didn't intend to delete. As well as an attacker can manipulate input data in a kind of injection attack.

> Overall, it's important to be cautious when using input methods, making sure that you have proper authentication and authorization in place to prevent unauthorized access. It's also a good idea to regularly audit your code and test for potential vulnerabilities. If trace and connect methods are not needed in production environment, so disable them.

### Discovering interesting files and folders on web servers

Discovering files and directories is usually the next task done after discovering a web service running on a target. And it is a kind of task that need to be automated. Nmap has a stript called `http-enum`, which is used to enumerate files and directories used by popular web applications.

There are several tools that perform this kind of task, and the main difference between them is the database, in which there are a combination of files names and directories. `Http-enum` has its own database, but allows to use nikto database, since a combination of popular directories and files names is set.

According to the script (`/usr/share/nmap/script`), only pages that return `200 OK` or `401 Authentication Required` are listed, except when `http-enum.displayall` is set. 

The first step of the script is to test if the server return a proper `404 Not Found` status, using two random files. In the case of different results, the script aborts because it is not possible distinguish between a 200-looking 404 cannot be distinguished and
an actual 200. If `301 Moved Permanently` or `401 Authentication Required`, then the script abort as well.

The basic command starts with: `nmap -sV --script http-enum scanme.nmap.org`. As result:

~~~ log
PORT      STATE    SERVICE    VERSION
22/tcp    open     ssh        OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.13 (Ubuntu Linux; protocol 2.0)
25/tcp    filtered smtp
80/tcp    open     http       Apache httpd 2.4.7 ((Ubuntu))
| http-enum: 
|   /images/: Potentially interesting directory w/ listing on 'apache/2.4.7 (ubuntu)'
|_  /shared/: Potentially interesting directory w/ listing on 'apache/2.4.7 (ubuntu)'
|_http-server-header: Apache/2.4.7 (Ubuntu)
1434/tcp  filtered ms-sql-m
9929/tcp  open     nping-echo Nping echo
31337/tcp open     tcpwrapped
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
~~~

By default, `http-enum` script uses the root directory, which is `/`. To change use `--script-args http-enum.basepath=`. Another useful args are: `--script-args http-enum.displayall` displays all file and directory tested, even the discarded ones. And `http-enum.fingerprintfile=`, which is used to define a specific fingerprint file.

### Brute forcing 

`http-brute` script is used to perform brute-force password auditing against web servers that are using http authentication, allowing robust dictionary attack against HTTP basic, digest, and NTLM authentication.

The basic command is: `nmap -p80 --script http-brute scanme.nmap.org`. As result:

~~~ log
~~~

By default, the script uses `usernames.lst` and `passwords.lst`, which are located at `/usr/shared/nmap/nselib/data/`. To change the reference lists, use `--script-args userdb=? usernames.txt,passdb=` arguments.

Another interesting argument is `--script-args brute.firstOnly`, which is used to quit after finding one valid account. 

It is possible to perform a brute-force password auditing against web applications, using databases of popular web applications, such as wordpress, joomla!, django, drupal, mediawiki, websphere.

The basic command is: `nmap --script http-form-brute -sV scanme.nmap.org`. as result:

~~~ log
~~~

The script automatically attempts to detect the form fields required to authenticate into the web application, and it uses a database of popular applications internally to help during the form detection phase. If script does not detected the fields, `--script-args http-form-brute.passvar=?,http-form-brute.uservar=?` arguments can be used to set them manually.

There are two specialized scripts for web application framework: `http-wordpress-brute` and `http-joomla-brute`. The joomla script will fetch a security token and include it in the login requests.

### Detecting web application firewalls

`http-waf-detect` and `http-waf-fingerprint` are scripts for detecting traffic filtering system such as web application firewall or intrusion prevention system. It can be done because because there are different responses in the web application when a malicious payload is used, then the script compare a response from a save http request with a response from a request that containing attack payload. The http-waf-detect script uses a fingerprint database to recognize special headers and cookies in the responses to attempt to identify products.

### Detecting possible XST vulnerabilities

`nmap -sV --script http-methods,http-trace --script-args http-methods.retest scanme.nmap.org`

### Detecting XSS vulnerabilities

`nmap -sV --script http-unsafe-output-escaping scanme.nmap.org`
`nmap -sV --script http-phpself-xss,http-unsafe-output-escaping scanme.nmap.org`
`nmap -sV --script http-xssed scanme.nmap.org` (query online database xssed.com)


### Finding SQL injection vulnerabilities

`nmap -sV --script http-sql-injection scanme.nmap.org`

### Finding web applications with default credentials

`nmap -sV --script http-default-accounts scanme.nmap.org`

For a less intrusive scan, filter out probes by category using `http-default-accounts.category`.

* web: this category manages web applications;
* router: this category manages the interfaces of routers;
* voip: this category contains voip devices;
* security: this category manages security-related software;
* industrial: this category manages software related to industrial control
systems (icses);
* printer: this category manages printer devices;
* storage: this category contains storage devices;
* virtualization: this category manages software for virtualization;
* console: this category contains remote consoles;

### Detecting insecure cross-domain policies

`nmap --script http-cross-domain-policy scanme.nmap.org`

### Detecting exposed source code control systems

`nmap -sV --script http-git scanme.nmap.org`
`nmap -sV --script http-svn-info scanme.nmap.org`

### Auditing the strength of cipher suites in SSL servers

`nmap --script ssl-enum-ciphers -sV scanme.nmap.org`

Other scripts: 

* ssl-ccs-injection: This checks whether a server is vulnerable to CCS injection (CVE-2014-0224).
* ssl-cert: This obtains information about SSL certificates.
* ssl-dh-params: This checks whether a server is vulnerable to Logham (CVE-2015-4000).
* ssl-heartbleed: This checks whether a server is vulnerable to Heartbleed (CVE-2014-0160).
* ssl-poodle: This checks whether a server is vulnerable to Poodle (CVE-2014-3566).
* sslv2-drown: This checks whether a server is vulnerable to Drown (CVE-2015-3197, CVE-2016-0703, and CVE-2016-0800);
* ssl-cert-intaddr: Reports any private IP address found in the SSL certificate;

## References

[RFC 9110 - HTTP Semantics](https://www.rfc-editor.org/rfc/rfc9110.html)

[developer.mozilla.org - HTTP](https://developer.mozilla.org/pt-BR/docs/Web/HTTP)

[wiki - Same Origin Policy](https://en.wikipedia.org/wiki/Same-origin_policy)

[owasp - Configuration_and_Deployment_Management_Testing/06-Test_HTTP_Methods](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/06-Test_HTTP_Methods)

[owasp - Cross_Site_Tracing](https://owasp.org/www-community/attacks/Cross_Site_Tracing)

[Cross site tracing xst the misunderstood vulnerability](https://deadliestwebattacks.com/appsec/2010/05/18/cross-site-tracing-xst-the-misunderstood-vulnerability)

[Three minutes with the http trace method](https://www.blackhillsinfosec.com/three-minutes-with-the-http-trace-method/)
