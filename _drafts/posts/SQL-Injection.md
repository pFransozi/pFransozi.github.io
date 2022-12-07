---
layout: post
title:  SQL Injection
date:   2022-08-21 16:40:16
description: analysis of SQL Injection technique
tags: Injection 
category: Cybersecurity Certification Content
---
The main topics of this story in order is:

What is SQL injection?
How do you find it?
How do you exploit it? (not yet finished)
How do you prevent it? (not yet finished)
References (not yet finished)

# 1 SQLi?
SQL injection, or just SQLi, is a kind of attack that exploits a vulnerability related to an attacker interfering with SQL commands made by an application to a database (SQL interpreter). In a specific situation, an attacker sends a payload that changes the programmed behavior of the SQL command inside the application. The successful SQLi attack can access (select) or modify (update, insert, delete) data that originally was not allowed, and also it can execute administrative operations that can compromise the DBMS, and back-end infrastructure, or it can perform a DoS attack.

In short, SQLi has the following components:

an input data, by which a trust payload is sent.
the code that handles the payload.
SQL interpreter, which interprets the SQL command with the payload.
Because of the payload and interpreter, SQLi is a subcategory of injection attacks. Nonetheless, it is also a code injection.

Let me put a historical dimension here using for it the OWASP Top Ten reports:

2004: A06: Injection Flaws.
2007: A02: Injection Flaws.
2010: A01: Injection.
2013: A01: Injection.
2017: A01: Injection.
2021: A03: Injection.
“Injection Flaws” and “Injection” categories include not just SQLi, but other types of injection attacks. According to the last report, the top CWE categories included in Injection are CWE-79: Cross-site Scripting, CWE-89: SQL Injection, and CWE-73: External Control of File Name or Path. And, for that, it is not clear what is the influence of SQLi alone is. Nevertheless, SQLi is mentioned at the top of the rank, and it is used as an example of how the attack is done and how to prevent it.

SQLi is common and a reason for that could be the fact that SQL database is common between applications, almost every application uses a database and consequently uses a SQL interpreter, which is a clue for an attacker. Eventually, an attacker can exploit some SQL interpreter vulnerability, which is not the usual scenario in this attack. Thus, those reports at least show how common the lack of sanitization of the payload is, how vast the area of trust is, or how common direct concatenation of string is to build SQL commands.

(Is it hard to detect SQLi by testing? Is it easy to forget the best implementation practices to prevent it? Is it a thing of lack of awareness about what is this vulnerability and how to prevent it?)

Let me return to the definition: the attacker can interfere with SQL commands because of two main misdrives in the application implementation:

The parameters, that come from the user-supplied inputs, interact with the SQL command directly through string concatenation.
There’s no proper sanitization of user-supplied inputs that are used in SQL command construction.
It sounds like the chicken or the egg causality dilemma, but just a little bit, because there are proper ways to handle each mistake that will be coved next.

More about SQLi, there is a way to classify SQLis according to the result or return. Some types of SQLi are:

retrieving hidden data, to return additional results in query, breaking the constraint of original SQL.
subverting Application Logic, to interfere with the application’s logic, for instance, changing a SQL command that validates user and password in a way that password is not considered.
union attacks, to add a union keyword, to retrieve data from different database tables, breaking the constraint of original SQL accessing other tables.
examining the database, to extract information about the version and structure of the database.
blind SQL injection, the query is not returned in the application’s responses.
Another classification is:

in-band: when an attacker uses the same communication channel to launch and gather the result of an attack. It is usually a simpler way to exploit SQLi vulnerability, one reason because the attacker can see the results as he moves forward in the attack. Related to this type is the Examing Database SQLi, also known as Error-Based SQLi, and Union-Based.
inferential (blind): it is a kind of reconnaissance, no data is transferred, and the attacker is able to analyze the behavior of the HTTP response and reconstruct the database structure, in an inferential way. There’re two types: boolean-based blind and time-based blind.
out-band: when an attacker is unable to use the same communication channel to launch and gather the result of an attack. Then, the attacker sends the malicious data that will be injected into the database server, forcing a way for the database server to send the result through another band, using DNS OR HTTP requests.
SQLi attacks can compromise the confidentiality, integrity, and availability of an application, as well as it can violate the triple-A (authentication, authorization, accounting), non-repudiation, and privacy. In other words, it can compromise and break all basic concepts in cybersecurity.

CWE-89: Improper Neutralization of Special Elements used in an SQL Command (‘SQL Injection’) is the Common Weakness Enumeration related to SQLi. Last CWE's Top 25 Most Dangerous Software Weaknesses, CWE-89 is among the three most dangerous, and the reason could be that it has become a common issue with database-driven websites, and it is a flaw that is easily detected, and easily exploited.

CWE-89 is a vulnerability related to data-rich applications that save user inputs in a database with data neutralization issues (CWE-699) and weakness related to the design and architecture of a system’s input validation component (CWE-1019).

Frequently these deal with sanitizing, neutralizing, and validating any externally provided inputs to minimize malformed data from entering the system and preventing code injection in the input data

The software constructs all or part of an SQL command using externally-influenced input from an upstream component, but it does not sanitize or incorrectly sanitizes special elements that could modify the intended SQL command when it is sent to a downstream component. Without sufficient removal or quoting of SQL syntax in user-controllable inputs, the generated SQL query can cause those inputs to be interpreted as SQL instead of ordinary user data. This can be used to alter query logic to bypass security checks, or to insert additional statements that modify the back-end database, possibly including execution of system commands

From CWE-89, it is easy to find all concrete vulnerabilities related to it by accessing the CVEs https://www.cvedetails.com/vulnerability-list/cweid-89/vulnerabilities.html.

# 2 How to find it?
There’re many tools that detect and exploit SQLi vulnerability, but before we go deeper into those tools, let’s understand the basics.

To break out a SQL query, a vulnerable parameter must be found, which means a parameter that is concatenated to a string query using direct string concatenation. Consider that a resource can be accessed through the following path: …/artists.php?artist=2. That path indicates a resource, which is “artists” and a parameter, which is “2”. By that path, the resource return information about artist=2.

It is very likely that the code that handles that resource uses a SQL command to select information about artist=2 from the database. There is a SQL interpreter at the end of the pipe. In the beginning, there is a data input that allows sending a payload inside the code that handles that resource, inside the SQL command that will be sent to the SQL interpreter.

SELECT name, age, last_album FROM artist where id_artist = 2;

The vulnerability is based on a lack of sanitization of the payload and on the use of direct string concatenation. And to discover that we have to try to force an error in the SQL interpreter, which means any modification here ‘id_artist = 2’ can cause an error.

An option is to use a single or double close quotation, like this artists.php?artist=” or artists.php?artist=’. If you get an unexpected behavior of the application, like an error, that means the SQL command is vulnerable.

For instance, let’s take a look at it using a LAB from PortSwigger Web Security Academy on SQL Injection. Using a filter from the web application,

Request URL: web-security-academy.net/filter?category=Lifestyle
Request Method: GET
Status Code: 200 OK

The payload is ‘Lifestyle’. Now, let's change to web-security-academy.net/filter?category=', and we can get an error from the server: 500 Internal Server Error.

#3 How to exploit it?

Exploring SQLi based on the types:

The following examples come from Web Security Academy from PortSwigger, link at references.

Retrieving Hidden Data

ORIGINAL: GET /filter?category=Food+%26+Drink

MODIFY: GET /filter?category=Food+%26+Drink%27+OR+1=1--

The original GET request sends to filter resource this parameter: category=Food+%26+Drink. “Food” and “Drink” are two categories in the online shop. “%26” is an encoded code from “&”.

The modified GET request includes at the end this: %27+OR+1=1 -- . “%27” means a single close quote. So the string is: ’+OR+1=1 -- .

The internal SQL command is something like this: string sql_cmd = “ […] WHERE category = ‘” + Request.form (“category”) + “’”;

Then converting the Request.form (“category”), it is: string sql_cmd = “ […] WHERE category = ‘” + Food&Drink’+OR+1=1-- “’”;

And then, the command is this: WHERE category = ‘Food&Drink’ OR 1=1 --’

The double dash is the comment keyword in SQL, so the last close quote is commented. and the expression “OR 1=1” turn the clause always true, returning all data.

Subverting Application Logic

ORIGINAL: &username=administrator&password=password

MODIFY: &username=administrator%27 -- %27&password=password

Union Attacks

The UNION keyword can retrieve data from other tables within the database when the application is vulnerable to SQLi. UNION keyword lets you execute one or more additional SELECT queries append to the original from the application and the result return append to the initial result.

For a UNION query to work, two key requirements must be met:

The individual queries must return the same number of columns.
The data types in each column must be compatible with the individual queries.
To meet those requirements:

How many columns are being returned from the original query?
Which columns returned from the original query are of a suitable data type to hold the results from the injected query?
Examining The Database

Blind SQL Injection

# 4 How prevent it?

# 5 References
* HOFFMAN, A. Web Application Security: Exploitation and Countermeasures for Modern Web Applications. O’Reilly: 2020.

* [PortSwigger, Web Security Academy](https://portswigger.net/web-security/learning-path)
* [OWASP TOP TEN 2021](https://owasp.org/Top10/)
* [OWASP — SQL Injection Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)
* [OWASP — SQL Injection](https://owasp.org/www-community/attacks/SQL_Injection)
* [OWASP-A03_2021-Injection](https://owasp.org/Top10/A03_2021-Injection/)
* [CWE-89: Improper Neutralization of Special Elements used in an SQL Command (‘SQL Injection’)](https://cwe.mitre.org/data/definitions/89.html)
* [SQLMap](https://github.com/sqlmapproject/sqlmap)
* [TML URL Encoding Reference](https://www.w3schools.com/tags/ref_urlencode.ASP)
