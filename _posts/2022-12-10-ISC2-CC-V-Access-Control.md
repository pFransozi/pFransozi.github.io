---
layout: post
title:  ISC² CC Access Control
date:   2022-12-10 8:50:00
description: analysis of third chapter of the self-paced “Certified in Cybersecurity” training, focusing on access control concepts; physical and logical access controls. 
tags: ['Access-Control', 'Concepts']
category: ['ISC²-CC-Content']
---
## Introduction

Here we continue ISC² CC foundational concepts, talking about access control. This is a sequence of a story started [in this link]({% post_url 2022-09-21-ISC2-CC-I-Foundational %}){:target="_blank"}.

In summary, access controls involve limiting what objects can be available to what subjects according to what rules. There are, in general, three components:

* **object** is a resource that is being protected, such as a file, a network, or a system;
* **subject** is a user or an entity that is requesting access to the object;
* **rules** is a set of conditions that determines whether a subject is granted access to an object;

At the end of the day, security all comes down to who can get access to organizational assets and what can they do when they get access.

Access controls are not just about restricting access to information system and data, but also about allowing access, which means to grant the appropriate level of access to authorized users, processes, services, and to deny access to unauthorized users, processes, services.

## Control Access Strategies

### defense in depth

Defense in depth describes a strategy that integrates people, technology and operations capabilities to establish variable barriers across multiple layers. It applies multiple countermeasures in a layered fashion to fulfill security objectives, such as prevent or deter a cyberattack, but it cannot guarantee that an attack will not occur. It follows some examples:

1. an username and password are required for logging in to an account, followed by a code sent to phone to verify user's identity. It's a form of multi-factor authentication using methods on two layers, something you have and something you know.
2. additional firewalls are used to separate untrusted networks with differing security requirements, such as the internet from trusted networks that house servers with sensitive data in the organization. When a company has information at multiple sensitivity levels, it might require the network traffic to be validated by rules on more than one firewall, with the most sensitive information being stored behind multiple firewalls.
3. consider the multiple layers of access required to get to the actual data in a data center. First, a lock on the door provides a physical barrier to access the data storage devices. Second, a technical access rule prevents access to the data via the network. Finally, a policy, or administrative control defines the rules that assign access to authorized individuals.

The idea behind defense in depth is to create a multi-layered security system that is resilient and can withstand multiple types of attacks. By implementing multiple layers of defense, it becomes more difficult for attackers to breach the system, and if one layer of defense is breached, there are still other layers in place to protect against the attack.

### principle of least privilege

The Principle of Least Privilege is a standard of permitting only minimum access necessary for users or programs to fulfill their function.

In general, related to PoLP is privileged access management (pam), which helps to preserve the confidentiality of information and ensure that it is only available to personnel who are authorized to see it, which means each user is granted access only to the items they need and nothing further. For example, only individuals working in billing will be allowed to view consumer financial data, and even fewer individuals will have the authority to change or delete that data. This maintains **confidentiality and integrity** while also **allowing availability** by providing administrative access with an appropriate password or sign-on that proves the user has the appropriate permissions to access that data.  

Systems often monitor access to private information, and if logs indicate that someone has attempted to access a database without the proper permissions, that will automatically trigger an alarm. The security administrator will then record the incident and alert the appropriate people to take action. The more critical information a person has access to, the greater the security should be around that access. They should definitely have multi-factor authentication, for instance.

The principle of least privilege is based on the idea that by limiting the access that users have, it becomes more difficult for attackers to compromise systems and networks. This can help to prevent data breaches, malware infections, and other types of security incidents.

### privileged access management

Privileged access management (PAM) is a security discipline that involves controlling and monitoring access to privileged accounts and sensitive systems. Privileged accounts are accounts that have more access and permissions than regular user accounts, such as administrator accounts and other accounts that have the ability to make changes to systems and networks. PAM involves implementing processes and technologies to ensure that access to privileged accounts is tightly controlled and monitored, in order to prevent unauthorized access and abuse. This can include implementing strong authentication mechanisms, such as two-factor authentication, to ensure that only authorized users are able to access privileged accounts. PAM can also involve implementing tools to monitor privileged account activity and detect potential security threats.

### segregation of duties

A core element of authorization is the principle of segregation of duties. It is based on the security practice that no one person should control an entire high-risk transaction from start to finish, which means it breaks the transaction into separate parts and requires a different person to execute each part of the transaction. Another implementation of segregation of duties is dual control, which means someone knows part of the secret and other person other, but never one of them know the full secret.

### privileged accounts

Privileged accounts are those with permissions beyond those of normal users, such as managers and administrators. Broadly speaking, these accounts have **elevated privileges** and are used by many different classes of users, including:

* **systems administrators**, who have the principal responsibilities for operating systems, applications deployment and performance management.
* help desk or it support staff, who often need to view or manipulate endpoints, servers and applications platforms by using privileged or restricted operations.
* security analysts, who may require rapid access to the entire IT infrastructure, systems, endpoints and data environment of the organization.

Other classes of privileged user accounts may be created on a per-client or per-project basis, to allow a member of that project or client service team to have greater control over data and applications. These few examples indicate that organizations often need to delegate the capability to manage and protect information assets to various managerial, supervisory, support or leadership people, with differing levels of authority and responsibility. This delegation, of course, **should be contingent upon trustworthiness**, since misuse or abuse of these privileges could lead to harm for the organization and its stakeholders. Typical measures used for moderating the potential for elevated risks **from misuse or abuse of privileged accounts include the following**:

* more extensive and detailed logging than regular user accounts;
* more stringent access control than regular user accounts;
* deeper trust verification than regular user accounts;
* more auditing than regular user accounts;

### how users are provisioned

Other situations that call for provisioning new user accounts or changing privileges include:

* a new employee;
* change of position;
* separation of employment;

## Physical Access Control

Physical access controls are self-explanatory. They are all things that can be physically touch, which means physical mechanisms deployed to prevent, monitor, or detect direct contact with systems or areas within a facility.

* Fencing or barriers around the perimeter of the datacenter property to prevent unauthorized entry;
* Gates or access points that can be opened or closed to control entry to the property;
* Security guards stationed at the gates or access points to verify the identity of individuals attempting to enter the property, using written authorization or visitor logs;
* Surveillance cameras or other monitoring systems to monitor activity around the gates and on the property;
* Access control panels or card readers at the entrances to the datacenter building that require authorized individuals to present a valid ID or security badge;
* Security doors or turnstiles that can be locked and unlocked remotely to control access to the interior of the datacenter building;
* Security cameras or other monitoring systems inside the datacenter building to monitor activity and detect unauthorized access;
* Biometric scanners or other advanced technology that can be used to verify the identity of individuals attempting to enter restricted areas of the datacenter, along with physical access logs or other written records of authorized access;

## Logical Access Control

Whereas physical access controls **are tangible methods or mechanisms that limit someone from getting access to an area or asset**, logical access controls are electronic methods that limit someone from getting access to systems, and sometimes even to tangible assets or areas.

Logical access control typically involves the use of user accounts, passwords, and other authentication methods to verify the identity of individuals who are attempting to access a computer system or network. Access to specific systems or data may be restricted based on the user's identity, their role or responsibilities within the organization, and the level of access that they are authorized to have.

Types of logical access controls include:

* **user accounts and passwords**: This is the most basic form of logical access control, and it involves creating unique user accounts for each individual who needs to access a computer system or network. Users must provide a valid username and password to authenticate their identity and gain access to the system;
* **two-factor authentication**: This is a more advanced form of logical access control that involves requiring users to provide two different forms of authentication in order to gain access to a system. In addition to a username and password, users may be required to provide a fingerprint, a security token, or some other form of authentication;
* **role-based access control**: In this type of logical access control, access to systems and data is granted based on the user's role or responsibilities within the organization. Different roles may have different levels of access to different systems and data, and access may be restricted or granted based on the user's role;
* **access control lists**: An access control list (ACL) is a list of users or groups of users who are granted or denied access to specific resources. The ACL can be used to control access to systems, data, or other resources on a network, and it can be used to enforce different levels of access for different users or groups;
* **context-based access control**: In this type of logical access control, access to systems and data is granted based on the context in which the user is accessing the system. This can include factors such as the user's location, the time of day, the device being used, and other contextual information;
* **discretionary access control**: In discretionary access control (DAC), access to systems and data is granted based on the user's discretion or judgment. This means that users have the ability to control who has access to the resources that they own or manage, and they can grant or revoke access as needed;
* **mandatory access control**: In mandatory access control (MAC), access to systems and data is granted based on a set of rules or policies that are defined by the system administrator. Access is not based on the user's discretion, and users do not have the ability to grant or revoke access to the resources that they own or manage;
* **rule-based access control**: In rule-based access control, access to systems and data is granted or denied based on a set of rules or policies that are defined by the system administrator. The rules can specify which users or groups of users are granted access to specific resources, and they can be used to enforce different levels of access based on the user's role or responsibilities.

## References

[Certified in Cybersecurity℠ — CC](https://www.isc2.org/Certifications/CC?filter=featured&searchRoot=A82B5ABE5FF04271998AE8A4B5D7DEFD){:target="_blank"}.

[NIST SP 800-53 Rev. 5](https://doi.org/10.6028/NIST.SP.800-53r5){:target="_blank"}.
