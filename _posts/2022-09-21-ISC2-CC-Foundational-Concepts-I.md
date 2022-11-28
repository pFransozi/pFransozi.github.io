---
layout: post
title:  ISC² CC Foundational Concepts I
date:   2022-08-21 16:40:16
description: analysis of foundational concepts from first chapter of the self-paced “Certified in Cybersecurity” training  
tags: ISC2 CC Concepts
category: Cybersecurity Certification Content
---

<p><strong>Introduction</strong></p>

From the beginning of the cybersecurity study, all students learn about some basic concepts, foundational ones, from where other ideas and guidelines come.

The first chapter of the self-paced “Certified in Cybersecurity” training is dedicated to discussing those cybersecurity foundational concepts and here some of them are covered as an exercise to understand them in a deeper way.

In summary, the topics are:

<ul>
    <li>Security concepts of information assurance: confidentiality, integrity, availability, authentication, authorization, accounting, non-repudiation, and privacy </li>
    <li>Risk management process: risk, probability x impact, asset, vulnerability, threat, risk identification, assessment, treatment, priorities, tolerance </li>
    <li>Security control: physical, technical, and administrative controls</li>
    <li>Governance: regulation, standard, policy, and procedure</li>
    <li>ISC² Code of Ethics</li>
</ul>

<p><strong>Confidentiality, Integrity, and Availability</strong></p>

Those three concepts compound the famous CIA triad and form the cornerstone of any organization’s security infrastructure and cybersecurity development. From those concepts, technologies are developed and upgraded to guarantee one or more requirements, which are placed by them, ensuring one or more aspects of what now is regarded as information security, or data security.

<b>Confidentiality</b> must guarantee data protection preventing access to unauthorized users and so improper data disclosure. But as well as it must guarantee authorized users access to information. From that derives gaining access to an information system (known as triple-A), non-repudiation, and privacy, for instance.

The difficulty to guarantee confidentiality is related to many users in the information system being guests or customers, and it is hard to define if the access comes from a compromised machine or vulnerable mobile application. The best practice is to regulate access to the information system, considering the level of sensitive information.

There are some groups of sensitive information: PII, PHI, and classified or sensitive information, which include trade secrets, research, business plans, and intellectual property.

* <b>Personal Identifiable Information</b> is any data that could be used to identify an individual;
* <b>Protected Health Information</b> comprehends information about one’s health status.

<b>Confidentiality</b> has a digital dimension, as well as a physical one, to which attackers look for physical sensitive materials. CC emphasizes it by showing the following threats:

* <b>Snooping</b> involves gathering information that is left out in the open. Clean desk policies protect against snooping;
* <b>Dumpster diving</b> also looks for sensitive materials, but in the dumpster, paper shredding protects against it;
* <b>Eavesdropping</b> occurs when someone secretly listens to a conversation, and it can be prevented with rules about sensitive conversations;
* <b>Wiretapping</b> is the electronic version of eavesdropping, the best way against that is to use encryption to protect the communication;
* <b>Social Engineering</b>, the best defense is to educate users to protect them against social engineering.

<b>Integrity</b> must guarantee the completeness, accuracy, internal consistency, and usefulness of data in an information system. Integrity is related to consistency and requires that all instances of the data be identical in form, content, and meaning.

<b>Integrity</b> must be guaranteed through all three states of data, which are at rest, in process, and in transit; and the best way to do that is by using cryptography.

Some threats are:

* <b>Unauthorized modification</b> attacks make changes without permission. The best way to protect against that is the least privilege principle;
* <b>Impersonation</b> attacks pretend to be someone else. User education protects against impersonation attacks.
* <b>Man-In-The-Middle (MITM)</b> attacks place the attacker in the middle of a communication session, monitoring everything that’s occurring.
* <b>Replay attacks</b> eavesdrop on logins and reuse the captured credentials.

<b>Availability</b> means that systems and data are accessible at the time and where authorized users need them in the form and format required. This does not mean that data or systems are available 100% of the time. Instead, the systems and data meet the requirements of the business for a timely and reliable access.

Some systems and data are far more critical than others. For that availability is constantly related to the term criticality, which represents how important data or information system is to an organization.

Some threats are:

* <b>Denial of Service</b> can be mitigated by using firewalls to block unauthorized connections.
* <b>Power outages</b> can be mitigated using redundant power and generators.
* <b>Hardware failures</b> can be mitigated using redundant components.
* <b>Destruction</b> can be mitigated using backups.
* <b>Service outages</b>.

<b>Confidentiality</b> requires some method of allowing access to authorized users and denying unauthorized ones, and that method is related to identification, authentication, and authorization.

Following the Security Principles module from CC, there are the following definitions:

<b>Authentication</b> is the process of access control, by which a user, after identifying himself, uses one or more factors to validate his identity. More and more, two or more factors (known as multi-factor authentication) are required, and they can be categorized as follow: something you know, something you have, or something you are.

<b>Non-repudiation</b> is the process through which an action can’t be denied by who performed that action. That can be achieved using proper ways of authentication and authorization, and traceability of the main transactions (accountability).

<b>Privacy</b> must guarantee the right of individuals to control the distribution of information, which is related to themselves.

<p><strong>References</strong></p>

* [Certified in Cybersecurity℠ — CC](https://www.isc2.org/Certifications/CC?filter=featured&searchRoot=A82B5ABE5FF04271998AE8A4B5D7DEFD)

* [What Is the CIA Triad?](https://www.f5.com/labs/learning-center/what-is-the-cia-triad#:~:text=These%20three%20letters%20stand%20for,objectives%20for%20every%20security%20program.)

* [Implementing the Health Insurance Portability and Accountability Act (HIPAA) Security Rule: A Cybersecurity Resource Guide](https://csrc.nist.gov/publications/detail/sp/800-66/rev-2/draft)