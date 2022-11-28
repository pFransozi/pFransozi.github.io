---
layout: post
title:  Broken Object Level Authorization
date:   2022-08-21 16:40:16
description: analysis of SQL Injection technique
tags: Injection 
category: Cybersecurity Certification Content
---

In 2019, OWASP released the first version of an API Security Top 10 report, which is similar to its other best-known project OWASP Top Ten. This year, OWASP plan to release a new version.

I decided to start an ongoing series of stories on API vulnerabilities, trying to understand how those vulnerabilities are exploited and why they are vulnerabilities, based on the OWASP report.

In this first part, the scope is Broken Object Level Authorization, which is also known just as BOLA.

Why BOLA becomes so common in modern apps, to the extent rank number one in the OWASP report? And how does an attacker exploit it?

#1 BOLA

BOLA is a technique that exploits authorization vulnerabilities and it is one of the most prominent API-related vulnerabilities, according to Ball and OWASP report.

Considering the OWASP report, BOLA almost reached the highest rank in all four categories.

Easy to exploit (highest rank 3);
Widespread prevalence (highest rank 3);
Average detectability (rank 2);
Severe technical impact (highest rank 3);
The architecture of modern applications grounds those two questions in the fact that those applications share a border through a network: the Internet; also called web API.

While an API, first of all, is a set of rules that allow communication between applications to share resources, an API is also a bunch of components that allow that. Between them, it focuses on these: endpoints, objects, and authorization control.

In this way, BOLA’s anatomy consists of: endpoints are manipulated by an attacker in a way to get access to an object that isn’t authorized, for in a traditional security model by default authenticated and authorized users are trusted, which means there is no authorization control at the level of the object.

Even though an attacker takes advantage of the modern architecture of the applications to perform the attack, the lack of authorization control at the object level seems to be the catalyst for this vulnerability. Or there is an authorization problem in the Business Logic Layer.

Just to be clear: to manipulate endpoints is not a vulnerability, but simply the state of art in web API communication; the lack of proper authentication is a vulnerability.

BOLA becomes so common in modern apps for these reasons:

On the client side, endpoints are easily manipulated either manually or through automation, mainly if parameters follow patterns, for instance, auto-incrementing IDs.
On the server side, the endpoint code, that handles the requests, relies on the incoming parameters rather than checking that the logged-in user is allowed to perform the requested action on the requested object. In other words, insecure coding practices.
Even more, the server side knows less about the client state, and for that, the client side must send additional parameters in each request to reflect its current state.
Based on those reasons, consider these points to difficult attacker’s action:

On the client side, make it difficult to identify patterns in the endpoints, using, for instance, UUID rather than auto-incrementing IDs.
On the server side, enforce a zero thrust model in the business logic layer; do not trust the parameters of a request merely because once the user has been authenticated and authorized. The authorization control must consider these questions: does the user have access to view the data? Does the user have access to modify the data? Does the user have access to delete the data?
That is: authorization must be enforced at the level of the object.

REFERENCES

* Ball, C. J. Hacking APIs. No Starch Press: 2022
* [Authorization Testing Automation Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Testing_Automation_Cheat_Sheet.html)
* [Authorization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html)
* [Architecting API Security for Adopting Zero Trust Model](https://www.youtube.com/watch?v=T3SN2p9xM38&ab_channel=OWASPBayArea)
* [What Is an API Endpoint? (And Why Are They So Important?)](https://blog.hubspot.com/website/api-endpoint#:~:text=It's%20important%20to%20note%20that,to%20retrieve%20the%20requested%20resources)
* [Insecure direct object references (IDOR)](https://portswigger.net/web-security/access-control/idor)
* [OWASP API Security Top 10 report](https://owasp.org/www-project-api-security/)
* [A Deep Dive On The Most Critical API Vulnerability — BOLA (Broken Object Level Authorization)](https://inonst.medium.com/a-deep-dive-on-the-most-critical-api-vulnerability-bola-1342224ec3f2)
* [What is Broken Object Level Authorization (BOLA) and How to Fix It](https://www.apisec.ai/blog/broken-object-level-authorization)
* [CWE-285: Improper Authorization](https://cwe.mitre.org/data/definitions/285.html)
* [CWE CATEGORY: Authorize Actors](https://cwe.mitre.org/data/definitions/1011.html)
* [Broken Object Level Authorization (BOLA)](https://www.wallarm.com/what/broken-object-level-authorization)
* [Golang Broken Object Level Authorization Guide: Examples and Prevention](https://www.stackhawk.com/blog/golang-broken-object-level-authorization-guide-examples-and-prevention/)
