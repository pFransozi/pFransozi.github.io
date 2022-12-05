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

ATT&CK is a huge behavioral model, which is defined by:

* **tactics**, denoting short-term, tactical adversary goals during an attack;
* **techniques**, describing the means by which adversaries achieve tactical goals;
* **Sub-techniques**, describing more specific means by which adversaries achieve tactical
goals at a lower level than techniques; and
* **documented** adversary usage of techniques, their procedures, and other metadata.

Currently, it has 14 tactics:

1. **reconnaissance**, which means that the attacker is gathering information, in a active or passive way, that can be used to support targeting, such as:
   * active scanning;
   * scanning IP blocks;
   * vulnerability scanning
   * wordlist scanning;
   * gather victim host information:
      hardware;
      software;
      firmware;
      client configuration;
   * gather victim identity information:
      * credentials;
      * email address;
      * employee names;
   * gather victim network information:
      * domain properties;
      * DNS;
      * network trust dependencies;
      * network topology;
      * IP addresses;
      * network security appliances;
   * gather victim org information:
      * determine physical locations;
      * business relationships;
      * identity business tempo;
      * identity roles;
   * phishing for information:
      * Spearphishing Service;
      * Spearphishing Attachment;
      * Spearphishing Link;
   * search closed sources:
      * Threat Intel Vendors;
      * Purchase Technical Data)
   * search open technical databases:
      * DNS/Passive DNS;
      * WHOIS;
      * Digital Certificates;
      * CDNs;
      * Scan Databases;
   * search open websites/domains:
      * Social Media;
      * Search Engines;
      * Code Repositories;
   * Search Victim-Owned Websites
2. **resource development**, which means that attacker is trying to establish resources that can be used to support operations, such as:
   * acquire infrastructure:
      * domains;
      * dns server;
      * virtual private server;
      * server;
      * botnet;
      * web services;
      * serverless;
   * compromise accounts:
      * social media accounts;
      * email accounts;
      * cloud accounts;
   * compromise infrastructure:
      * domains;
      * dns server;
      * virtual private server;
      * server;
      * botnet;
      * web services;
      * serverless;
   * develop capabilities:
      * malware;
      * code signing certificates;
      * digital certificates;
      * exploits;
   * establish accounts:
      * social media accounts;
      * email accounts;
      * cloud accounts;
   * obtain capabilities:
      * malware;
      * tool;
      * code signing certificates;
      * digital certificates;
      * exploits;
      * vulnerabilities;
   * stage capabilities:
      * upload malware;
      * Upload tool;
      * install digital certificate;
      * drive-by target;
      * link target;
      * seo poisoning;
3. **initial access**, which means various entry vectors to gain the first foothold into the network, such as:
   * drive-by compromise;
   * exploit public-facing application;
   * external remote services;
   * hardware additions;
   * phishing:
      * spearphishing attachment;
      * spearphishing link;
      * spearphishing via service;
   * replication through removable media;
   * supply chain compromise:
      * compromise software dependencies and development tools;
      * compromise software supply chain;
      * compromise hardware supply chain;
   * trusted relationship;
   * valid accounts:
      * default accounts;
      * domain accounts;
      * local accounts;
      * cloud accounts;
4. Execution, which means adversary-controlled code running on a local or remote system, aiming broader goals, like exploring a network or stealing data, such as:
   * command and scripting interpreter:
      * powershell;
      * applescript;
      * windows command shell;
      * unix shell;
      * visual basic;
      * python;
      * javascript;
      * network device cli;
   * container administration command;
   * deploy container;
   * exploitation for client execution;
   * inter-process communication:
      * component object model;
      * dynamic data exchange;
      * xpc services;
   * native api
   * scheduled task/job:
      * at;
      * cron;
      * scheduled task;
      * systemd timers;
      * container orchestration job;
   * serverless execution
   * shared modules
   * software deployment tools
   * system services:
      * launchctl;
      * service execution;
   * user execution:
      * malicious link;
      * malicious file;
      * malicious image;
   * windows management instrumentation

5. Persistence, which consists fo techniques that adversaries use to keep access to systems across restarts, changed credentials, and other interruptions that could cut off their access, such as:
   * Account Manipulation (Additional Cloud Credentials, Additional Email Delegate Permissions, Additional Cloud Roles, SSH Authorized Keys, Device Registration);
   * BITS Jobs;
   * Boot or Logon Autostart Execution (Registry Run Keys / Startup Folder, Authentication Package, Time Providers, Winlogon Helper DLL, Security Support Provider, Kernel Modules and Extensions, Re-opened Applications, LSASS Driver, Shortcut Modification, Port Monitors, Print Processors, XDG Autostart Entries, Active Setup, Login Items);
   * Boot or Logon Initialization Scripts (Logon Script (Windows), Login Hook, Network Logon Script, RC Scripts, Startup Items);
   * Browser Extensions;
   * Compromise Client Software Binary;
   * Create Account (Local Account, Domain Account, Cloud Account);
   * Create or Modify System Process (Launch Agent, Systemd Service, Windows Service, Launch Daemon);
   * Event Triggered Execution (Change Default File Association, Screensaver, Windows Management Instrumentation Event Subscription, Unix Shell Configuration Modification, Trap, LC_LOAD_DYLIB Addition, Netsh Helper DLL, Accessibility Features, AppCert DLLs, AppInit DLLs, Application Shimming, Image File Execution Options Injection, PowerShell Profile, Emond, Component Object Model Hijacking, Installer Packages);
   * External Remote Services;
   * Hijack Execution Flow (DLL Search Order Hijacking, DLL Side-Loading, Dylib Hijacking, Executable Installer File Permissions Weakness, Dynamic Linker Hijacking, Path Interception by PATH Environment Variable, Path Interception by Search Order Hijacking, Path Interception by Unquoted Path, Services File Permissions Weakness, Services Registry Permissions Weakness, COR_PROFILER, KernelCallbackTable);
   * Implant Internal Image;
   * Modify Authentication Process (Domain Controller Authentication, Password Filter DLL, Pluggable Authentication Modules, Network Device Authentication, Reversible Encryption, Multi-Factor Authentication, Hybrid Identity);
   * Office Application Startup (Office Template Macros, Office Test, Outlook Forms, Outlook Home Page, Outlook Rules, Add-ins);
   * Pre-OS Boot (System Firmware, Component Firmware, Bootkit, ROMMONkit, TFTP Boot);
   * Scheduled Task/Job (At, Cron, Scheduled Task, Systemd Timers, Container Orchestration Job);
   * Server Software Component (SQL Stored Procedures, Transport Agent, Web Shell, IIS Components, Terminal Services DLL);
   * Traffic Signaling (Port Knocking, Socket Filters);
   * Valid Accounts (Default Accounts, Domain Accounts, Local Accounts, Cloud Accounts)
6. Privilege Escalation;

   * Abuse Elevation Control Mechanism (Setuid and Setgid, Bypass User Account Control, Sudo and Sudo Caching, Elevated Execution with Prompt);
   * Access Token Manipulation (Token Impersonation/Theft, Create Process with Token, Make and Impersonate Token, Parent PID Spoofing, SID-History Injection);
   * Boot or Logon Autostart Execution (Registry Run Keys / Startup Folder, Authentication Package, Time Providers, Winlogon Helper DLL, Security Support Provider, Kernel Modules and Extensions, Re-opened Applications, LSASS Driver, Shortcut Modification, Port Monitors, Print Processors, XDG Autostart Entries, Active Setup, Login Items)
   * boot or logon initialization scripts:
      * logon script (windows);
      * login hook;
      * network logon script;
      * rc scripts;
      * startup items;
   * create or modify system process:
      * Launch Agent;
      * Systemd Service;
      * Windows Service;
      * Launch Daemon;
   * domain policy modification:
      * group policy modification;
      * domain trust modification;
   * escape to host:
   * event triggered execution:
      * Change Default File Association
      * Screensaver
      * Windows Management Instrumentation Event Subscription
      * Unix Shell Configuration Modification
      * Trap
      * LC_LOAD_DYLIB Addition
      * Netsh Helper DLL
      * Accessibility Features
      * AppCert DLLs
      * AppInit DLLs
      * Application Shimming
      * Image File Execution Options Injection
      * PowerShell Profile
      * Emond
      * Component Object Model Hijacking
      * Installer Packages
   * Exploitation for Privilege Escalation
   * Hijack Execution Flow
      * DLL Search Order Hijacking
      * DLL Side-Loading
      * Dylib Hijacking
      * Executable Installer File Permissions Weakness
      * Dynamic Linker Hijacking
      * Path Interception by PATH Environment Variable
      * Path Interception by Search Order Hijacking
      * Path Interception by Unquoted Path
      * Services File Permissions Weakness
      * Services Registry Permissions Weakness
      * COR_PROFILER
      * KernelCallbackTable
   * Process Injection
      * Dynamic-link Library Injection
      * Portable Executable Injection
      * Thread Execution Hijacking
      * Asynchronous Procedure Call
      * Thread Local Storage
      * Ptrace System Calls
      * Proc Memory
      * Extra Window Memory Injection
      * Process Hollowing
      * Process Doppelgänging
      * VDSO Hijacking
      * ListPlanting
   * Scheduled Task/Job
      * At
      * Cron
      * Scheduled Task
      * Systemd Timers
      * Container Orchestration Job
   * Valid Accounts
      * Default Accounts
      * Domain Accounts
      * Local Accounts
      * Cloud Accounts

7. Defense Evasion, which means techniques used to avoid detection throughout their compromise, such as
   * Abuse Elevation Control Mechanism:
      * Setuid and Setgid;
      * Bypass User Account Control;
      * Sudo and Sudo Caching
      * Elevated Execution with Prompt
   * Access Token Manipulation:
      * Token Impersonation/Theft;
      * Create Process with Token;
      * Make and Impersonate Token;
      * Parent PID Spoofing;
      * SID-History Injection;
   * BITS Jobs;
   * Build Image on Host;
   * Debugger Evasion;
   * Deobfuscate/Decode Files or Information;
   * Deploy Container;
   * Direct Volume Access;
   * Domain Policy Modification:
      * Group Policy Modification
      * Domain Trust Modification
   * Execution Guardrails:
      * Environmental Keying
   * Exploitation for Defense Evasion
   * File and Directory Permissions Modification
      * Windows File and Directory Permissions Modification
      * Linux and Mac File and Directory Permissions Modification
   * Hide Artifacts
      * Hidden Files and Directories
      * Hidden Users
      * Hidden Window
      * NTFS File Attributes
      * Hidden File System
      * Run Virtual Instance
      * VBA Stomping
      * Email Hiding Rules
      * Resource Forking
      * Process Argument Spoofing
   * Hijack Execution Flow
       * DLL Search Order Hijacking
       * DLL Side-Loading
       * Dylib Hijacking
       * Executable Installer File Permissions Weakness
       * Dynamic Linker Hijacking
       * Path Interception by PATH Environment Variable
       * Path Interception by Search Order Hijacking
       * Path Interception by Unquoted Path
       * Services File Permissions Weakness
       * Services Registry Permissions Weakness
       * COR_PROFILER
       * KernelCallbackTable
   * Impair Defenses
       * Disable or Modify Tools
       * Disable Windows Event Logging
       * Impair Command History Logging
       * Disable or Modify System Firewall
       * Indicator Blocking
       * Disable or Modify Cloud Firewall
       * Disable Cloud Logs
       * Safe Mode Boot
       * Downgrade Attack
   * Indicator Removal
      * Clear Windows Event Logs
      * Clear Linux or Mac System Logs
      * Clear Command History
      * File Deletion
      * Network Share Connection Removal
      * Timestomp
      * Clear Network Connection History and Configurations
      * Clear Mailbox Data
      * Clear Persistence
   * Indirect Command Execution
   * Masquerading
       * Invalid Code Signature
       * Right-to-Left Override
       * Rename System Utilities
       * Masquerade Task or Service
       * Match Legitimate Name or Location
       * Space after Filename
       * Double File Extension
   * Modify Authentication Process
       * Domain Controller Authentication
       * Password Filter DLL
       * Pluggable Authentication Modules
       * Network Device Authentication
       * Reversible Encryption
       * Multi-Factor Authentication
       * Hybrid Identity
   * Modify Cloud Compute Infrastructure
       * Create Snapshot
       * Create Cloud Instance
       * Delete Cloud Instance
       * Revert Cloud Instance
   * Modify Registry
   * Modify System Image
       * Patch System Image
       * Downgrade System Image
   * Network Boundary Bridging
      * Network Address Translation Traversal
   * Obfuscated Files or Information
       * Binary Padding
       * Software Packing
       * Steganography
       * Compile After Delivery
       * Indicator Removal from Tools
       * HTML Smuggling
       * Dynamic API Resolution
       * Stripped Payloads
       * Embedded Payloads
   * Plist File Modification
   * Pre-OS Boot
       * System Firmware
       * Component Firmware
       * Bootkit
       * ROMMONkit
       * TFTP Boot
   * Process Injection
       * Dynamic-link Library Injection
       * Portable Executable Injection
       * Thread Execution Hijacking
       * Asynchronous Procedure Call
       * Thread Local Storage
       * Ptrace System Calls
       * Proc Memory
       * Extra Window Memory Injection
       * Process Hollowing
       * Process Doppelgänging
       * VDSO Hijacking
       * ListPlanting
   * Reflective Code Loading
   * Rogue Domain Controller
   * Rootkit
   * Subvert Trust Controls
       * Gatekeeper Bypass
       * Code Signing
       * SIP and Trust Provider Hijacking
       * Install Root Certificate
       * Mark-of-the-Web Bypass
       * Code Signing Policy Modification
   * System Binary Proxy Execution
       * Compiled HTML File
       * Control Panel
       * CMSTP
       * InstallUtil
       * Mshta
       * Msiexec
       * Odbcconf
       * Regsvcs/Regasm
       * Regsvr32
       * Rundll32
       * Verclsid
       * Mavinject
       * MMC
   * System Script Proxy Execution
      * PubPrn
   * Template Injection
   * Traffic Signaling
      * Port Knocking
      * Socket Filters
   * Trusted Developer Utilities Proxy Execution
      * MSBuild
   * Unused/Unsupported Cloud Regions
   * Use Alternate Authentication Material
      * Application Access Token
      * Pass the Hash
      * Pass the Ticket
      * Web Session Cookie
   * Valid Accounts
      * Default Accounts
      * Domain Accounts
      * Local Accounts
      * Cloud Accounts
   * Virtualization/Sandbox Evasion
      * System Checks
      * User Activity Based Checks
      * Time Based Evasion
   * Weaken Encryption
      * Reduce Key Space
      * Disable Crypto Hardware
   * XSL Script Processing
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
