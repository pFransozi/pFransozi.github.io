---
layout: post
title:  ISC² CC IV Incident Response, Business Continuity and Disaster Recovery Concepts
date:   2022-12-08 21:30:00
description: analysis of second chapter of the self-paced “Certified in Cybersecurity” training, focusing on incident response, business continuity and disaster recovery.
tags: ['incident-response', 'business-continuity', 'disaster-recovery', 'concepts']
category: ['ISC²-CC-Content']
---
## Introduction

Here we continue ISC² CC from second chapter of the self-paced "Certified in Cybersecurity". The first chapter start [here]({% post_url 2022-09-21-ISC2-CC-I-Foundational %}){:target="_blank"}.

This post is based on three topics:

* **incident response**;
* **business continuity**;
* **disaster recovery**;

## Incident Response Basic Aspects

Let's try some definitions:

1. it is a plan that is used to respond to unexpected changes in operating conditions to keep the business operating;

2. it is **the process** of responding to and managing the aftermath of a security breach or cyber attack, involving a series of steps to contain the breach, minimize damage, and start the recovery process. It includes activities such as identifying the cause of the incident, determining its scope, collecting evidence, taking corrective action, and communicating with stakeholders;

Based on those two definitions, let's try draw some propositions:

1. as in every cybersecurity activities, incident response is an ongoing process;
2. even though it is a response plan, it requires awareness of the operation conditions and mechanisms to detect unexpected changes in it;
3. the critical events are security breach or cyber attack
4. some steps: contain the breach, minimize damage, start the recovery process
5. some activities: identifying the cause of the incident, determining its scope, collecting evidence, taking corrective action, communicating with stakeholders.

In every field of knowledge there are terms that describe that field and support definitions, the incident terminology is:

* **breach** is an event of loss of control, unauthorized disclosure, unauthorized acquisition, or any similar occurrence, where a person other than an authorized user accesses or potentially accesses personally identifiable information;
* **event** is any observable occurrence** in a network or system;
* **exploit** is a particular attack that exploits system vulnerabilities;
* **incident** is an event that actually or potentially jeopardizes the confidentiality, integrity or availability of an information system or the information the system processes, stores or transmits;
* **intrusion** is a security event, or a combination of events, that constitutes a deliberate security incident in which an intruder gains, or attempts to gain, access to a system or system resource without authorization;
* **threat** is any circumstance or event with the potential to adversely impact organizational operations, organizational assets, individuals, other organizations or the nation through an information system via unauthorized access, destruction, disclosure, modification of information and/or denial of service;
* **vulnerability** is weakness in an information system, system security procedures, internal controls or implementation that could be exploited by a threat source;
* **zero day** is a previously unknown system vulnerability with the potential of exploitation without risk of detection or prevention because it does not, in general, fit recognized patterns, signatures or methods;

### the goal of incident response

The priority of IR is to protect life, health and safety, which means always to choose safety first. The primary goal of incident management is to be prepared, which requires having a policy and a response plan that will lead the organization through the crisis. A plan avoids to be taken by surprise at unexpected events.

This ongoing process is also called crisis management and it must help preserve business viability and survival. The incident response process is aimed at reducing the impact of an incident so the organization can resume the interrupted operations as soon as possible. Note that incident response planning is a subset of the greater discipline of business continuity management (BCM).

### components of the incident response plan

The plan may contain several procedures and standards related to incident response, that is defined by the organization’s vision, strategy and mission, for those aspects must define the risk management process. Procedures to implement the plan should define the technical processes, techniques checklists and other tools that teams will use when responding to an incident.

CC defines four steps, as follows:

* **preparation** is divided into several aspects, such as:
  * develop a policy approved by management;
  * identify critical data and systems, single points of failure;
  * training staff on incident response;
  * implement an incident response team;
  * practice incident identification;
  * plan the coordination of communication between stakeholders, considering the possibility that a primary method of communication may not be available.
* **detection and analysis** must :
  * monitor all possible attack vectors;
  * analyze incident using known data and threat intelligence;
  * prioritize incident response;
  * standardize incident documentation;
* **containment, eradication and recovery**:
  * gather evidence;
  * choose an appropriate containment strategy;
  * identify the attacker;
  * isolate the attack;
* **post-incident activity**:
  * identify evidence that may need to be retained;
  * document lessons learned;
  * retrospective, preparation, detection and analysis containment;
  * eradication and recovery post-incident activity.

### incident response team

After all, an organization has two main needs:

* establish a security operation center (SOC);
* create a suitable incident response team;

A typical incident response team is a cross-functional group of individuals who represent the management, technical and functional areas of responsibility most directly impacted by a security incident.

Potential team members include the following:

* representative(s) of senior management;
* information security professionals;
* legal representatives;
* public affairs/communications representatives;
* engineering representatives (system and network);

Typically, team members assist with:

* investigating the incident;
* assessing the damage;
* collecting evidence;
* reporting the incident;
* initiating recovery procedures.
* participate in the remediation and lessons learned stages and help with root cause analysis.

These teams are commonly known as computer incident response teams (CIRTs) or computer security incident response teams (CSIRTs).

When an incident occurs, the response team has four primary responsibilities:

* determine the amount and scope of damage caused by the incident;
* determine whether any confidential information was compromised during the incident;
* implement any necessary recovery procedures to restore security and recover from incident-related damage;
* supervise the implementation of any additional security measures necessary to improve security and prevent recurrence of the incident;

## Business Continuity

Business continuity plan intents to sustain business operations while recovering from a significant disruption.

CC highlights communication as key part of the plain, including:

* multiple contact methodologies;
* backup numbers in case of a disruption of power or communications;
* establish a phone tree, so that if one person is not available, they know who else to call;

Since priorities may change depending on the situations, individuals with proper authority must be in the situation to execute critical operations.

### components of a business continuity plan

Business continuity planning is the proactive development of procedures to restore business operations after a disaster or other significant disruption to the organization, and must ensure that all systems, processes and operations are accounted for in the plan. In order to safeguard the confidentiality, integrity and availability of information, the technology must align with the business needs.

* list of the BCP team members, including multiple contact methods and backup members;
* immediate response procedures and checklists (security and safety procedures, fire suppression procedures, notification of appropriate emergency-response agencies, etc.);
* notification systems and call trees for alerting personnel that the BCP is being enacted;
* guidance for management, including designation of authority for specific managers;
* how/when to enact the plan. It's important to include when and how the plan will be used;
* contact numbers for critical members of the supply chain (vendors, customers, possible external emergency providers, third-party partners).

### business continuity plan tests

How often should an organization test its business continuity plan (BCP)?

This is a critical question that must be done, and the answer is routinely, constantly, ongoing process. Each individual organization must determine how often to test its BCP, but it should be tested at predefined intervals as well as when significant changes happen within the business environment.

## Disaster Recovery

Disaster recovery planning steps in where BC leaves off.

Disaster recovery refers specifically to restoring the information technology and communications services and systems needed by an organization. The recovery of a business function may be done independently of the recovery of IT and communications services; however, the recovery of IT is often crucial to the recovery and sustainment of business operations.

Whereas business continuity planning is about maintaining critical business functions, disaster recovery planning is about restoring IT and communications back to full operations after a disruption.

### components of a disaster recovery plan

* executive summary providing a high-level overview of the plan;
* department-specific plans;
* technical guides for IT personnel responsible for implementing and maintaining critical backup systems;
* full copies of the plan for critical disaster recovery team members;
* checklists for certain individuals:
  * critical disaster recovery team members will have checklists to help guide their actions amid the chaotic atmosphere of a disaster;
  * it personnel will have technical guides helping them get the alternate sites up and running;
  * managers and public relations personnel will have simple-to-follow, high-level documents to help them communicate the issue accurately without requiring input from team members who are busy working on the recovery.
* executive management should approve the plan and should be provided with a high-level summary of the plan.
* public Relations should be a member of the disaster recovery plan to handle communications to all stakeholders.
* it Personnel are primarily responsible for the disaster recovery team.

## References

* [Certified in Cybersecurity℠ — CC](https://www.isc2.org/Certifications/CC?filter=featured&searchRoot=A82B5ABE5FF04271998AE8A4B5D7DEFD){:target="_blank"}.

* [Contingency Planning Guide for Federal Information Systems](https://csrc.nist.gov/publications/detail/sp/800-34/rev-1/final){:target="_blank"}.

* [Computer Security Incident Handling Guide](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final){:target="_blank"}.

* [Guidelines to plan and prepare for incident response](https://www.iso.org/standard/62071.html){:target="_blank"}.

* [Guidelines for information and communication technology readiness for business continuity](https://www.iso.org/obp/ui/#iso:std:iso-iec:27031:ed-1:v1:en){:target="_blank"}.
