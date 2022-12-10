---
layout: post
title:  ISC² CC Foundational Concepts I
date:   2022-11-28 11:33:00
description: analysis of foundational concepts from first chapter of the self-paced “Certified in Cybersecurity” training, focusing on CIA.
tags: ['ISC2', 'CC', 'Concepts', 'CIA']
category: ['Cybersecurity-Certification-Content']
---

## Introduction

**ISC² CC Foundational Concepts** is a bunch of stories based on self-paced course called **Certified in Cybersecurity from (ISC)²**, which is a preparation for (ISC)² **Certified in Cybersecurity**, which is an entry-level cybersecurity certification.

The main domains are:

* **Security Principles**;
* **Business Continuity**;
* **Disaster Recovery & Incident Response Concepts**;
* **Access Controls Concepts**;
* **Network Security**;
* **Security Operations**.

The course objectives are:

* to recognize and to discuss foundational security concepts of cybersecurity principles and information assurance;
* to define and summarize risk management terminology and its process, relating risk management to personal or professional practices;
* to classify types of security controls;
* to distinguish between policies, procedures, standards, regulations and laws;
* to demonstrate the relationship among governance elements;
* to explain how organizations respond to, recover from and continue to operate during unplanned disruptions; to recall the terms and components of incident response; to summarize the components of a business continuity plan; to identify the components of disaster recovery; to practice the terminology and review concepts of business continuity, disaster recovery and incident response;
* to select access controls that are appropriate in a given scenario; to relate access control concepts and processes to given scenarios; to compare various physical access controls; to describe logical access controls; to practice the terminology and review concepts of access controls;
* to explain the concepts of network security; to recognize common networking terms and models; to identify common protocols and port and their secure counterparts; to identify types of network (cyber) threats and attacks; to identify secure network design terminology; to practice the terminology and review concepts of network security;
* to discuss common tools used to identify and prevent threats;
* to identify common data center terminology and to recognize common cloud service terminology;
* to explain concepts of security operations and to discuss data handling best practices, such as: to identify key concepts of logging and monitoring, to summarize the different types of encryption and their common uses, to describe the concepts of configuration management;
* to explain the application of common security policies;
* to discuss the importance of security awareness training;
* to practice the terminology and review concepts of network operations.

My main aim with these stories are:

* to prepare for the cybersecurity certification, regarding with attention all objectives from the course;
* to review some foundational aspects of cybersecurity field;
* to share this knowledge.

From the beginning of the cybersecurity study, all students learn about some basic concepts, foundational ones, from where other ideas and guidelines come.

The first chapter of the self-paced “Certified in Cybersecurity” training is dedicated to discussing those cybersecurity foundational concepts and here some of them are covered as an exercise to understand them in a deeper way.

**In summary**, the topics are:

* **Security concepts of information assurance**
  * confidentiality;
  * integrity;
  * availability;
  * authentication;
  * authorization;
  * accounting;
  * non-repudiation;
  * privacy.
* **Risk management process**
  * risk;
  * probability x impact;
  * asset, vulnerability;
  * threat;
  * risk identification;
  * assessment;
  * treatment;
  * priorities;
  * tolerance.
* **Security control**
  * physical;
  * technical;
  * administrative controls.
* **Governance**
  * regulation;
  * standard;
  * policy;
  * procedure.
* **ISC² Code of Ethics**

## Confidentiality, Integrity, and Availability

Those three concepts compound the famous CIA triad and form the cornerstone of any organization’s security infrastructure and cybersecurity development. From those concepts, technologies are developed and upgraded to guarantee one or more requirements, which are placed by them, ensuring one or more aspects of what now is regarded as information security, or data security.

### Confidentiality

It must guarantee data protection preventing access to unauthorized users and so improper data disclosure. But as well as it must guarantee authorized users access to information. From that,it derives gaining access to an information system (known as triple-A), non-repudiation, and privacy, for instance.

The difficulty to guarantee confidentiality is related to many users in the information system being guests or customers, and it is hard to define if the access comes from a compromised machine or vulnerable mobile application. The best practice is to regulate access to the information system, considering the level of sensitive information.

There are some groups of sensitive information, and the main are PII and PHI. Additionally, classified or sensitive information, which include trade secrets, research, business plans, and intellectual property are considered sensitive information.

* **Personal Identifiable Information** is any data that could be used to identify an individual;
* **Protected Health Information** comprehends information about one’s health status.

**Confidentiality** has a digital dimension, as well as a physical one, to which attackers look for physical sensitive materials. CC emphasizes it by showing the following threats:

Related to confidentiality are:

* **sensitive information**;
* **closed or disclosed information**;
* **privacy**;
* **integrity**.

* **Snooping** involves gathering information that is left out in the open. Clean desk policies protect against snooping;
* **Dumpster diving** also looks for sensitive materials, but in the dumpster, paper shredding protects against it;
* **Eavesdropping** occurs when someone secretly listens to a conversation, and it can be prevented with rules about sensitive conversations;
* **Wiretapping** is the electronic version of eavesdropping, the best way against that is to use encryption to protect the communication;
* **Social Engineering**, the best defense is to educate users to protect them against social engineering.

## Integrity

It must guarantee the completeness, accuracy, internal consistency, and usefulness of data and systems in an information system. Integrity is related to consistency and requires that all instances of the data be identical in form, content, and meaning.

**Integrity** must be guaranteed through all the three states of data, which are at rest, in process, and in transit; and the best way to do that is by using cryptography. And as well as through entire lifetime of systems and processes. But for that, **it must be known the integral state of those**. It is critical to ensure integrity awareness of the integral state of data, systems, processes. To achieve that level, it is highly recommended the creation of a baseline.Baseline means documents that register the integral and consistent state of the information systems, which requires awareness of the information system.

Related to integrity are:

* **consistency**;
* **awareness**;
* **baseline**.

Some threats are:

* **Unauthorized modification** attacks make changes without permission. The best way to protect against that is the least privilege principle;
* **Impersonation** attacks pretend to be someone else. User education protects against impersonation attacks.
* **Man-In-The-Middle (MITM)** attacks place the attacker in the middle of a communication session, monitoring everything that’s occurring.
* **Replay** attacks eavesdrop on logins and reuse the captured credentials.

## Availability

It means that systems and data are accessible at the time and where authorized users need them in the form and format required. This does not mean that data or systems are available 100% of the time. Instead, the systems and data meet the requirements of the business for a timely and reliable access.

Some systems and data are far more critical than others. For that availability is constantly related to the term criticality, which represents how important data or information system is to an organization. And consequently, how available it must be.

Related to availability are:

* to authorized users when and where it is needed and in the form and format required;
* criticality and the different level of it among data and systems.

Some threats are:

* **Denial of Service** can be mitigated by using firewalls to block unauthorized connections.
* **Power outages** can be mitigated using redundant power and generators.
* **Hardware failures** can be mitigated using redundant components.
* **Destruction** can be mitigated using backups.
* **Service outages**.

## A little bit more

**Confidentiality** requires some method of allowing access to authorized users and denying unauthorized ones, and that method is related to identification, authentication, and authorization. Those are known as Triple-A and are the three steps to gain access.

* **identification** is the action of to claim of identity and validate that the claimed identity belong into the system.
* **authentication** is the action of to validate that the identity is owned by who claimed the identity, by comparing one or more factor of authentication. When one it is known as Single-factor authentication, SFA, when more than one, Multi-factor authentication, MFA. More and more, two or more factors are required, and they can be categorized as follow: something you know, something you have, something you are, where you are.
  * Password is still the most common method of authentication. The password must follow some requirements:
    * length requirements set a minimum number of chars;
    * complexity requirements describe the types of characters that must be included;
    * expiration requirements force password changes. Nowadays, that requirement isn't used, companies change to an approach where force password change is required when there is any evidence that the password has been compromised;
    * history requirements prevent password reuse;
    * provide a way to change the password quickly and easily;
    * encourage users to not reuse the same password across multiple sites;
    * password managers facilitate the use of strong, unique password
* **authorization**: after identifying and authentication, authorization ensures that the user of the information system uses only allowed resources;
* **accounting**: its maintains logs of activity;
* **non-repudiation**: it is the process through which an action can’t be denied by who performed that action. That can be achieved using proper ways of authentication and authorization, and traceability of the main transactions (accountability).
* **Privacy** must guarantee the right of individuals to control the distribution of information, which is related to themselves.

## References

* [Certified in Cybersecurity℠ — CC](https://www.isc2.org/Certifications/CC?filter=featured&searchRoot=A82B5ABE5FF04271998AE8A4B5D7DEFD){:target="_blank"}

* [What Is the CIA Triad?](https://www.f5.com/labs/learning-center/what-is-the-cia-triad#:~:text=These%20three%20letters%20stand%20for,objectives%20for%20every%20security%20program.){:target="_blank"}

* [Implementing the Health Insurance Portability and Accountability Act (HIPAA) Security Rule: A Cybersecurity Resource Guide](https://csrc.nist.gov/publications/detail/sp/800-66/rev-2/draft){:target="_blank"}
