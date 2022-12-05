---
layout: post
title:  THM AoC 2022 DAY ONE
date:   2022-12-01 09:00:00
description: walking through the first day's topics, and diving deeper
tags: ["NIST-CSF", "ISO-27001", "ISO-27002", "MITRE-ATT&CK", "Cyber-Kill-Chain", "Unified-Kill-Chain"]
category: ["Try-Hack-Me", "THM-AoC-2022", "Security-Framework"]
---

## Introduction

TryHackMe's first day of Advent of Cyber 2022 is here, and we have to face theory about security frameworks, such as NIST Cybersecurity Framework, ISO 27001 e 27002, MITRE ATT&CK, Cyber Kill Chain, Unified Kill Chain. The latter can be described as the unification of the ATT&CK and Kill Chain frameworks, and it is the focus of the first day.

## Security Frameworks

### What is a security framework?

Let's divide the expression "security framework", and understand the word "framework". Framework is widely used in different areas, but semantically there is one thing in common, which is a basic structure underlying another one, supporting it in an essential way.

Thus, a security framework is a basic structure that support the security operation, defining policies, control objectives, standards, controls, procedures, guidelines, baselines. In general guidelines and baselines are discretionary, which means there is a margin for judgment. On the other hand, policies, control objectives, standards, controls, procedures are mandatory, which means no margin for judgment.

Then, security framework is a set of documents that define a basic structure which underlie all the security processes, in a way to identify and manage the risks and the weaknesses. In general, the modern security frameworks are risk- and awareness-aproach.

### Why we have to know and follow frameworks?

We have to know and follow security frameworks because they reduce or even remove guesswork to the extent that they include processes and structures in a strategic plan that involves the security objectives.

Follow a security framework is not an one-time activity, but instead it is a set of long-term-ongoing activities. Security framework is not an activity just when a security incident occurs, but it has to be present on the scene before, during and after.

Additionally, as a consequence, it will help achieve commercial and government regulatory requirements.

### And more

Notwithstanding the technical and business side, frameworks provide a common language between the stakeholders, helping communication and understanding.

Before starting, a hint: the content below is just a summary, it's highly recommended study one of them or all frameworks for a grounded understanding. 

### NIST Cybersecurity Framework

CSF is a risk-based framework, focusing on manage cybersecurity risk. It is composed of three parts:

* **core**: it presents industry standards, guidelines, and practices. The Core consists of five concurrent and continuous functions: identity, protect, detect, respond, recover.
* **implementation tiers**: they provide context on how an organization views cybersecurity risk and the processes in place to manage that risk. Context means the degree to which an organization's cybersecurity risk management practices exhibit the characteristics defined in the Framework. The degree is defined as: partial (T1) to adaptive (T4), which means a progression from informal to approaches that are agile and risk-informed. During this phase, the organization must consider the current state-of-the-art, which includes: its current risk management practices, threat environment, legal and regulatory requirements, business/mission objectives, and organizational constraints.
* **profiles**: it is the alignment, based on the selected framework categories and subcategories, of standards, guidelines, and practices to the framework core in a particular implementation scenario. A profile is developed based on business/mission drivers and a risk assessment, determining which categories and subcategories are most important in a way to address the organization's risks. Profiles can be used to support prioritization and measurement of progress toward the target profile, to conduct self-assessments and to communicate.

#### Core

Core is defined by five functions, which arrange basic cybersecurity activities, organizing information, enabling risk management decisions, addressing threats, and improving by learning from previous activities. They are aligned with existing methodologies for incident management
and help show the impact of investments in cybersecurity. Each function is described by:

* **categories** subdivide functions into groups of cybersecurity outcomes closely tied to programmatic needs and particular activities, such as "asset management", "awareness and training", "security continuous monitoring", "analysis", "recovery planning".
* **subcategories** subdivide categories into specific outcomes of technical or management activities such as "data-at-rest is protected", "notifications from detection systems are investigated"
* **information references** are specific standards, guidelines, practices that illustrate a method to achieve the outcomes associated with each subcategory.

And the five functions are:

* **identity**: it is related essentially to understanding and awareness about the systems, people, assets, data, and capabilities, with the risks related to them. Examples are: asset management, business environment, governance, risk assessment, and risk management strategy.
* **protect**: it is related essentially to ensuring delivery of critical services, limiting or containing the impact of a potential cybersecurity event. Examples are: identity management and access control, awareness and training, data security, information protection processes and procedures, maintenance, and protective technology.
* **detect**: it is related essentially to identifying the occurrence of a cybersecurity event. Examples are: anomalies and events, security continuous monitoring, and detection processes.
* **respond**: it is related essentially to taking action regarding a detected cybersecurity incident. Examples are: response planning, communications, analysis, mitigation; and improvements.
* **recover**: it is related essentially to maintaining plans for resilience and to restoring any capabilities or services that were impaired due to a cybersecurity incident.

Before ending or overview, let's describe the steps to create a CSF program. NIST's document ranks seven steps, such as:

1. **prioritize and scope**: to identify business objectives and high-level organizational priorities; to make strategic decisions regarding cybersecurity implementations and to determine the scope of systems and assets that support the selected business line or process; to adapt the framework to support different business lines or processes, which may have different business needs and risk tolerance.
2. **orient**: once prioritize and scope has been determined, to identity related systems and assets, regulatory requirements, and overall risk approach; to consult sources to identity threats and vulnerabilities applicable to those systems and assets.
3. **create a current profile**: then, to develop a current profile, which indicates categories and subcategories from the Framework Core that are currently being achieved.
4. **conduct a risk assessment**: to analyze the operational environment in order to discern the likelihood of a cybersecurity event and the impact that the event could have on the organization; to identify emerging risks and use cyber threat information from internal and external sources to gain a better understanding of the likelihood and impact of cybersecurity events.
5. **create a target profile**: to creates a Target Profile that focuses on the assessment of the Framework Categories and Subcategories describing the organization’s desired cybersecurity outcomes; to develop an additional Categories and Subcategories to account for unique organizational risks; to consider influences and requirements of external stakeholders such as sector entities, customers, and business partners when creating a Target Profile. The Target Profile should appropriately reflect criteria within the target Implementation Tier.
6. **determine, analyze, and prioritize gaps**: to compare the Current Profile and the Target Profile to determine gaps; next, to create a prioritized action plan to address gaps, reflecting mission drivers, costs and benefits, and risks, to achieve the outcomes in the Target Profile; to determine resources, including funding and workforce, necessary to address the gaps. Using Profiles in this manner encourages the organization to make informed decisions about cybersecurity activities, supports risk management, and enables the organization to perform cost-effective, targeted improvements.
7. **implement action plan**: to determine which actions to take to address the gaps, if any, identified in the previous step and then adjusts its current cybersecurity practices in order to achieve the Target Profile. For further guidance, to determine which standards, guidelines, and practices, including those that are sector specific, work best for their needs.

It is an ongoing process and an organization repeats the steps as needed to continuously assess and improve its cybersecurity.

### ISO 27001 and ISO 27002

The first day of Advent of Cyber 2022 of TryHackMe mentions briefly ISO 27001 and 27002, which are standards for cybersecurity and outline the requirements and procedures for creating, implementing and managing an information security management system (ISMS).

ISO 27001:2022, the latest version, is a specification for an information security management system (ISMS), which means a framework of policies and procedures that includes all legal, physical and technical controls involved in an information risk management processes.

> Provide a model for establishing, implementing, operating, monitoring, reviewing, maintaining and improving an information security management system.

It's technology-neutral, risk-based-approach, helping organizations manage their information security by addressing people, processes, and technology. For that, it defines six planning process, which are:

1. Define a security policy;
2. Define the scope of the ISMS;
3. Conduct a risk assessment;
4. Manage identified risks;
5. Select control objectives and controls to be implemented;
6. Prepare a statement of applicability.

It does not mandate specific information security controls, but it provides a checklist of controls that should be considered in the accompanying code of practice, ISO/IEC 27002:2005.

ISO 27002:2022, the latest version, provides best practice on information security control for use by those responsible for initiating, implementing or maintaining ISMS in the context of the CIA triad.

> The preservation of confidentiality (ensuring that information is accessible only to those authorized to have access), integrity (safeguarding the accuracy and completeness of information and processing methods) and availability (ensuring that authorized users have access to information and associated assets when required).

ISO 27002 contains 12 main sections:

1. Risk assessment;
2. Security policy;
3. Organization of information security;
4. Asset management;
5. Human resources security;
6. Physical and environmental security;
7. Communications and operations management;
8. Access control;
9. Information systems acquisition, development and maintenance;
10. Information security incident management;
11. Business continuity management;
12. Compliance.

### MITRE ATT&CK Framework

ATT&CK is behavioral model, which is defined by:

* **tactics**, denoting short-term, tactical adversary goals during an attack;
* **techniques**, describing the means by which adversaries achieve tactical goals;
* **Sub-techniques**, describing more specific means by which adversaries achieve tactical
goals at a lower level than techniques; and
* **documented** adversary usage of techniques, their procedures, and other metadata.

Currently, it has 14 tactics:

1. Reconnaissance;
2. Resource Development;
3. Initial Access;
4. Execution;
5. Persistence;
6. Privilege Escalation;
7. Defense Evasion;
8. Credential Access;
9. Discovery;
10. Lateral Movement;
11. Collection;
12. Command and Control;
13. Exfiltration;
14. Impact.

Each of that tactic is divided into techniques and procedures (TTP).

### Cyber Kill Chain

It describes the structure of an attack and consists of target identification, decision and order to attack the target, and finally, target destruction. The cyber kill chain describes the stages commonly followed by cyberattacks and security defenders can use the framework as part of intelligence-driven defense.

1. Recon
2. Weaponizations
3. Delivery
4. Exploitation
5. Installation
6. Command & Control
7. Actions on Objectives

### Unified Kill Chain

Unified Kill Chain: The Unified Kill Chain can be described as the unification of the MITRE ATT&CK and Cyber Kill Chain frameworks. The Unified Kill Chain describes 18 phases of attack based on Tactics, Techniques and Procedures (TTPs). The individual phases can be combined to form overarching goals, such as gaining an initial foothold in a targeted network, navigating through the network to expand access and performing actions on critical assets. Santa’s security team would need to understand how these phases are put together from the attacker’s perspective.

## References

### THM resources

[THM Learning path about Cyber Defence Frameworks](https://tryhackme.com/module/cyber-defence-frameworks){:target="_blank"}.

[THM Room about Unified Kill Chain](https://tryhackme.com/room/unifiedkillchain){:target="_blank"}.

[THM Room about Cyber Kill Chain](https://tryhackme.com/room/cyberkillchainzmt){:target="_blank"}.

[THM Room about MITRE](https://tryhackme.com/room/mitre){:target="_blank"}.

[THM Room about MITRE](https://tryhackme.com/room/mitre){:target="_blank"}.

### NIST Cybersecurity Framework resources

[NIST Cybersecurity Framework](https://www.nist.gov/cyberframework){:target="_blank"}.

[OAS AWS NIST CSF](https://www.oas.org/en/sms/cicte/docs/OAS-AWS-NIST-Cybersecurity-Framework(CSF)-ENG.pdf){:target="_blank"}.

### MITRE ATT&CK Framework resources

[ATTACK's main page](https://attack.mitre.org/){:target="_blank"}

[ATTACK Design and Philosophy](https://attack.mitre.org/docs/ATTACK_Design_and_Philosophy_March_2020.pdf){:target="_blank"}

### Cyber Kill Chain resources

[Cyber Kill Chain](https://www.lockheedmartin.com/en-us/capabilities/cyber/cyber-kill-chain.html)

### Unified Kill Chain resources
