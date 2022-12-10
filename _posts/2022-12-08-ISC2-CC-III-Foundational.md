---
layout: post
title:  ISC² CC Foundational Concepts III
date:   2022-12-08 8:00:00
description: analysis of foundational concepts from first chapter of the self-paced “Certified in Cybersecurity” training, focusing on security controls and governance elements.
tags: ['security-control', 'governance-elements', 'concepts']
category: ['ISC²-CC-Content']
---
## Introduction

This is the last part of the first chapter from the self-paced “Certified in Cybersecurity” training, which is about Security Controls and Governance Elements. The first two parts are:

* [ISC2 CC Foundational Concepts I]({% post_url 2022-09-21-ISC2-CC-I-Foundational %}){:target="_blank"}
* [ISC2 CC Foundational Concepts II]({% post_url 2022-11-29-ISC2-CC-II-Foundational %}){:target="_blank"}

## Security Control

Always with the purpose of protecting confidentiality, integrity and availability of the information systems, security controls are measures or actions that are used for that purpose, and they can be physical, technical and administrative.

* **physical controls** are measures that use physical barriers or other physical means to protect a system or network. These controls can include things like locks, gates, or security guards. An example of a physical control would be using a lock to prevent unauthorized access to a server room, or having security guards patrol a building to prevent unauthorized entry.

* **technical controls** (also called logical controls) are measures that use technology to protect a system or network from unauthorized access or attacks. These controls can include things like firewalls, intrusion detection systems, and encryption technologies. An example of a technical control would be requiring a password to access a computer system, or using encryption to protect sensitive data. The implementation of technical controls always requires significant operational considerations and should be consistent with the management of security within the organization.

* **administrative controls** (also known as managerial controls) are measures that are put in place to manage and control access to a system or network. These controls can include things like policies, procedures, and user training. An example of an administrative control would be requiring employees to undergo security training, or implementing policies that dictate how sensitive data can be accessed and used. They provide frameworks, constraints and standards for human behavior, and should cover the entire scope of the organization’s activities and its interactions with external parties and stakeholders. It is vitally important to realize that administrative controls **can and should be powerful, effective tools for achieving information security**. Even the simplest security awareness policies can be an effective control, if you can help the organization fully implement them **through systematic training and practice**.

It is important to consider that those three security controls for the most part must work together to protect a system or network in the real world. Imagine, for instance, that a company has a server room that contains sensitive data and resources. To protect this server room, the company implements several security controls.

* **first**, they install a technical control in the form of a firewall, which prevents unauthorized access to the network.
* **next**, they implement a physical control by installing locks on the doors to the server room, which prevents unauthorized entry.
* **finally**, they put in place an administrative control by requiring all employees who need access to the server room to undergo security training and obtain a security badge.

In this scenario, the technical control provides the technical means to secure the network, the physical control provides the physical barriers to prevent unauthorized access, and the administrative control provides the policies and procedures to manage and control access to the server room. Together, these three types of security controls work to protect the company's sensitive data and resources from unauthorized access or attacks.

## Governance Elements

In the context of cybersecurity, governance elements refer to the policies, procedures, and practices that are put in place to manage and control access to a computer system or network, and to protect it from unauthorized access or attacks. Governance elements are an important part of ensuring the security of a system or network, and they can help to prevent unauthorized access or damage to sensitive data or resources.

* **regulations** are legal requirements that dictate how organizations must handle and protect sensitive data and systems. For example, in the United States, the Health Insurance Portability and Accountability Act (HIPAA) is a regulation that requires organizations in the healthcare industry to protect the confidentiality and security of patient information.
* **standards** are guidelines or benchmarks that organizations can use to assess and improve their security practices. For example, the ISO/IEC 27001 standard is a widely-used standard that provides a framework for implementing and managing an effective information security management system.
* **policies** are rules and guidelines that organizations put in place to manage and control access to their systems and data. For example, a company might have a policy that requires employees to use strong passwords and to change them regularly.
* **procedures** are specific steps or actions that organizations follow in order to achieve a particular goal. For example, an organization might have a procedure for conducting regular security audits, in which it checks its systems and networks for vulnerabilities or weaknesses.

Some examples of those are:

* **access control policies** are policies that dictate who is allowed to access specific resources or systems, and what level of access they are granted. Access control policies can help to prevent unauthorized access to sensitive data or systems.
* **incident response procedures** are procedures that outline the steps to be taken in the event of a security incident or breach. Incident response procedures can help to minimize the impact of a security incident and ensure that it is handled in a timely and effective manner.
* **security training** provides regular training to employees and other users can help to educate them about the importance of security, and to teach them how to protect themselves and the organization's systems and data.
* **security policies** are policies that outline the rules and guidelines for using a computer system or network, and that describe what is and is not allowed. Security policies can help to prevent security incidents and protect against threats.

Overall, regulations, standards, policies, and procedures are all important tools for managing and protecting computer systems and networks in the cybersecurity context. They provide the framework and guidance that organizations need to ensure the security of their systems and data, and to prevent unauthorized access or attacks.

## References

* [Certified in Cybersecurity℠ — CC](https://www.isc2.org/Certifications/CC?filter=featured&searchRoot=A82B5ABE5FF04271998AE8A4B5D7DEFD){:target="_blank"}

* [Security and Privacy Controls for Information Systems and Organizations](https://csrc.nist.gov/publications/detail/sp/800-53/rev-5/final){:target="_blank"}

* [NIST Cybersecurity Framework](https://www.nist.gov/cyberframework){:target="_blank"}.
