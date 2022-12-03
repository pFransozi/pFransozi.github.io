---
layout: post
title:  THM AoC 2022 DAY ONE
date:   2022-12-01 09:00:00
description: walking through the first day's topics, and diving deeper
tags: ["THM", "AoC2022", "NIST CSF", "ISO 27000 Series", "MITRE ATT&CK", "Cyber Kill Chain", "Unified Kill Chain"]
category: ["THM AoC2022", "Security Framework"]
---

## Introduction

First day of Advent of Cyber 2022 of TryHackMe and we have to face theory about security frameworks, such as NIST Cybersecurity Framework, ISO 27000 Series, MITRE ATT&CK Framework, Cyber Kill Chain, Unified Kill Chain. The latter can be describe as the unification of the MITRE ATT&CK and Cyber Kill Chain frameworks, and it is the focus of the first day.

## Security Frameworks

### What is a security framework?

Let's divide the expression "security framework", and understand the word "framework". Framework is widely used in different areas, but semantically there is one thing in common, which is a basic structure underlying another one, supporting it in a essential way.

Thus, a security framework is a basic structure that support the security operation, defining policies, control objectives, standards, controls, procedures, guidelines, baselines. In general guidelines and baselines are discretionary, which means there is a margin for judgment. On the other hand, policies, control objectives, standards, controls, procedures are mandatory, which means no margin for judgment.

Then, security framework is a set of documents that define a basic structure which underlie all the security processes, in a way to identify and manage the risks and the weaknesses.

### Why we have to know and follow frameworks?

We have to know and follow security frameworks because they reduce or even remove guesswork to the extent that include processes and structures in a strategic plan that involves the security objectives.

Follow a security framework is not an one-time activity, but instead it is a set of long-term-ongoing activities. Security framework is not an activity just when a security incident occurs, but it has to be present on the scene before, during and after.

Additionally, as a consequence, it will help achieve commercial and government regulatory requirements.

### And more

Notwithstanding the technical and business side, frameworks provide a common language between the stakeholders, helping communication and understanding.

### NIST Cybersecurity Framework

NIST Cybersecurity Framework can be found [in this link](https://www.nist.gov/cyberframework){:target="_blank"}, in which there are others important links, such as the page to the Journey to CSF 2.0, which is the new version of the framework, not yet released.

Let's take a brief look at the CSF.

CSF is a risk-based framework, focusing on manage cybersecurity risk. It is composed of three parts:

* **core**: it presents industry standards, guidelines, and practices. The Core consists of five concurrent and continuous functions: identity, protect, detect, respond, recover.
* **implementation tiers**: they provides context on how an organization views cybersecurity risk and the processes in place to manage that risk. Context means the degree to which an organization's cybersecurity risk management practices exhibit the characteristics defined in the Framework. The degree is define as: partial (T1) to adaptive (T4), which means a progression from informal to approaches that are agile and risk-informed. During this phase, the organization must consider the current state-of-the-art, which includes: its current risk management practices, threat environment, legal and regulatory requirements, business/mission objectives, and organizational constraints.
* **profiles**:  it is the alignment, based on the selected framework categories and subcategories, of standards, guidelines, and practices to the framework core in a particular implementation scenario. A profile is developed based on business/mission drivers and a risk assessment, determining which categories and subcategories are most important in a way to address the organization's risks. Profiles can be used to support prioritization and measurement of progress toward the target profile, to conduct self-assessments and to communicate.

#### Core

Core is defined by five functions, which arrange basic cybersecurity activities, organizing information, enabling risk management decisions, addressing threats, and improving by learning from previous activities. They are aligned with existing methodologies for incident management
and help show the impact of investments in cybersecurity. Each function is described by: 

* **categories** subdivide functions into groups of cybersecurity outcomes closely tied to programmatic needs and particular activities, such as "asset management", "awareness and training", "security continuous monitoring", "analysis", "recovery planning".
* **subcategories** subdivide categories into specific outcomes of technical or management activities such as "data-at-rest is protected", "notifications from detection systems are investigated"
* **information references** are specific standards, guidelines, practices that illustrate a method to achieve the outcomes associated with each subcategory.

And the five functions are:

* **identity**: it is related essentially to understanding and awareness of the systems, people, assets, data, and capabilities, with the risks related to them. Examples are: asset management, business environment, governance, risk assessment, and risk management strategy.
* **protect**: it is related essentially to ensuring delivery of critical services, limiting or containing the impact of a potential cybersecurity event. Examples are: identity management and access control, awareness and training, data security, information protection processes and procedures, maintenance, and protective technology.
* **detect**: it is related essentially to identifying the occurrence of a cybersecurity event. Examples are: anomalies and events, security continuous monitoring, and detection processes.
* **respond**: it is related essentially to taking action regarding a detected cybersecurity incident. Examples are: response planning, communications, analysis, mitigation; and improvements.
* **recover**: it is related essentially to maintaining plans for resilience and to restoring any capabilities or services that were impaired due to a cybersecurity incident.

Before ending or overview, let's describe the steps to create a CSF program. NIST's document ranks seven steps, such as:

1. **prioritize and scope**
2. **orient**
3. **create a current profile**
4. **conduct a risk assessment**
5. **create a target profile**
6. **determine, analyze, and prioritize gaps**
7. **implement action plan**

This is just a summary of NIST's document and it's highly recommended study all resources about CSF for a grounded understanding.

### ISO 27000 Series

The ISO 27001 and 27002 standards are commonly known for cybersecurity and outline the requirements and procedures for creating, implementing and managing an information security management system (ISMS).

### MITRE ATT&CK Framework

The MITRE ATT&CK framework is a knowledge base of TTPs (Tactics, Techniques and Procedures ), carefully curated and detailed to ensure security teams can identify attack patterns. It brings together environment-specific cybersecurity information to provide cyber threat intelligence insights that help teams develop effective security programs for their organisations.

### Cyber Kill Chain

Cyber Kill Chain: which describes the structure of an attack and consists of target identification, decision and order to attack the target, and finally, target destruction. the cyber kill chain describes the stages commonly followed by cyber attacks and security defenders can use the framework as part of intelligence-driven defence.

1. Recon
2. Weaponization
3. Delivery
4. Exploitation
5. Installation
6. Command & Control
7. Acions on Objectives

### Unified Kill Chain

Unified Kill Chain: The Unified Kill Chain can be described as the unification of the MITRE ATT&CK and Cyber Kill Chain frameworks. The Unified Kill Chain describes 18 phases of attack based on Tactics, Techniques and Procedures (TTPs). The individual phases can be combined to form overarching goals, such as gaining an initial foothold in a targeted network, navigating through the network to expand access and performing actions on critical assets. Santa’s security team would need to understand how these phases are put together from the attacker’s perspective.

## References

### THM resources

[THM LearningPath about Cyber Defence Frameworks](https://tryhackme.com/module/cyber-defence-frameworks){:target="_blank"}.

[THM Room about Unified Kill Chain](https://tryhackme.com/room/unifiedkillchain){:target="_blank"}.

[THM Room about Cyber Kill Chain](https://tryhackme.com/room/cyberkillchainzmt){:target="_blank"}.

[THM Room about MITRE](https://tryhackme.com/room/mitre){:target="_blank"}.

[THM Room about MITRE](https://tryhackme.com/room/mitre){:target="_blank"}.

### NIST Cybersecurity Framework resources

[NIST Cybersecurity Framework](https://www.nist.gov/cyberframework){:target="_blank"}.

[OAS AWS NIST CSF](https://www.oas.org/en/sms/cicte/docs/OAS-AWS-NIST-Cybersecurity-Framework(CSF)-ENG.pdf){:target="_blank"}.

### ISO 27000 Series resources

### MITRE ATT&CK Framework resources

### Cyber Kill Chain resources


### Unified Kill Chain resources

