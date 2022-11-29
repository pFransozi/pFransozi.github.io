---
layout: post
title:  ISC² CC Foundational Concepts II
date:   2022-11-29 10:56:00
description: analysis of foundational concepts from first chapter of the self-paced “Certified in Cybersecurity” training, focusing on risk management process
tags: ISC2 CC Concepts Risk
category: Cybersecurity Certification Content
---

## Introduction

Here we'll continue ISC² CC foundational concepts, talking about risk management process concepts. This is a sequence of a story started [in this link]({% post_url 2022-09-21-ISC2-CC-Foundational-Concepts-I %}){:target="_blank"}.

Before we reach our target, let's talk a little bit about concepts and risk management process.

**Why do we have to learn about concepts?** Frankly, we do not have, except if we want to have tools to handle the chaos of reality. Concepts help us to see reality less chaotic, helping us to manage real-life situations, both the most ordinary and the most extraordinary.

Our scenarios are on risk management in a cybersecurity context and nowadays it means any risk that **can affect confidentiality, integrity and availability of a business**. For that, risk management must become a process, **an ongoing concern**, in any organization, by which any risk must be assess and analyze **in a continuous and comprehensive way**. And in a security team, the work pass through risk assessment, analysis, mitigation, remediation and communication.

So, this story covers:

* **risk management terminology**;
* **risk identification**;
* **risk assessment**;
* **risk treatment**;
* **risk priorities**;
* **risk tolerance**.

## Risk Management Terminology

Discuss terminology is a way to delimit the scope of a topic, since any scope of a topic is delimited by the terms, the concepts that ground it. It's tautology! Furthermore, it is a way to put everybody inside that tautology, setting up a common language.

* **risk** is a measure of the extent to which an entity is threatened by a **potential** circumstance or event. It is often expressed as a combination of:
  * the adverse impacts that would arise if the circumstance or event occurs
  * the likelihood of occurrence.
* **asset** is something in need of protection because it has value to the organization. It could be a tangible asset or intangible, such as information;
* **vulnerability** is a gap or **weakness** in protection of valuable assets, including information. A vulnerability is an inherent weakness or flaw in a system or component, which, if triggered or acted upon, **could cause a risk event to occur**. **An organization’s security team strives to identify and to decrease its vulnerability**. For that, it is critical has the pointer of view of an attacker: **Why would we be an attractive target Managing vulnerabilities starts with one simple step: Learn what they are**;
* **a threat** is something or someone that **aims to exploit a vulnerability to gain unauthorized access**;
* **a likelihood** of a **potential vulnerability** being **exploited** within the construct of the organization’s threat environment. Likelihood of occurrence is a weighted factor **based on a subjective analysis** of the **probability** that a given threat or set of threats is capable of exploiting a given vulnerability or set of vulnerabilities;
* **mitigation** takes action to prevent or reduce the impact of an event;
* **acceptance** ignores the risks and continuing risky activities;
* **avoidance** ceases the risky activity to remove the likelihood that an event will occur;
* **transference** passes risk to a third party.

## Risk Identification

**Important consider** that it's not **a one-and-done activity**, but it's a **recurring process** of identifying different possible risks, characterizing them, and then estimating their potential for disrupting the organization. This process is crucial to expose possible risks because **what is not known cannot be protected**. And employees at all levels of the organization are responsible for identifying risk.

## Risk Assessment

It's the process of identifying, estimating, and prioritizing risks. Risk assessment has a broad meaning in comparison to risk identification because the risk assessment aligns each identified risk resulting from the operation of an information system with the organization's goals and objectives.

## Risk Treatment

It is the intersection between prioritized risk and decisions about the best actions to take regarding the identified risk. The decisions made are dependent on the attitude of management toward risk and the availability — and cost — of risk mitigation. The question that guide the decision is: **what is the best action to take regarding the identified and prioritized risks?**

The options commonly used to respond to risk are four. Let's see:

* **avoidance**: it is the decision to attempt to eliminate the risk entirely, including ceasing operation for some or all activities exposed to the particular risk. Organization leadership may choose risk avoidance when the potential impact of a given risk is too high or if the likelihood of the risk being realized is simply too great;
* **acceptance**: it is taking no action to reduce the likelihood of a risk occurring and still conducting the business function that is associated with the risk without any further action. Either because the impact or likelihood of occurrence is negligible, or because the benefit is more than enough to offset that risk.
* **mitigation**: it is the most common type of risk management and includes taking actions to prevent or reduce the possibility of a risk event or its impact. Mitigation can involve remediation measures or controls, such as security controls, establishing policies, procedures, and standards to minimize adverse risk. Risk cannot always be mitigated, but mitigation as safety measures should always be in place.
* **transfer**: it is the practice of passing the risk to another party, who will accept the financial impact of the harm resulting from a risk being realized in exchange for payment. This is an insurance policy.

## Risk Priorities

After risk identification phase, it is time to analyze and prioritize what emerge from prior phase. The analysis focus on qualitative aspect as well as on quantitative one. This is necessary to determine **root cause and narrow down apparent risks and core risks**.

The risk priorities phase requires an understanding of the organizations's overall mission and the functions that support the mission **in a way to place risks in context**, determining the root causes and then prioritizing the assessment and analysis.

One effective method to prioritize risk is to use **a risk matrix**, which means an intersection of likelihood of occurrence and business impact.
<br>

|   Probability  | X |   Impact  |
|:--------------:|:-:|:---------:|
|low             |   |low        |
|low             |   |high       |
|high            |   |low        |
|high            |   |high       |

<br>
As resulting: low - low results in a low priority, while as incident with a high likelihood and high impact will result in a high priority. Assignment of priority may relate to business priorities, the cost of mitigating a risk or the potential for loss if an incident occurs.

When making decisions based on risk priorities, organizations must evaluate the likelihood and impact of the risk as well as their tolerance for different sorts of risk. determining risk tolerance is up to the executive management and board of directors. If a company chooses to ignore or accept risk, exposing workers to asbestos, for example, it puts the company in a position of tremendous liability.

## Risk Tolerance

How much risk are they willing to take? Does management welcome risk or want to avoid it? Those questions indicate the risk tolerance and the level of risk tolerance varies across organizations, and even internally: Different departments may have different attitudes toward what is acceptable or unacceptable risk.

for that, understanding the organization and senior management’s attitude toward risk is usually the starting point for getting management to take action regarding risks.

## References

[NIST SP 800–30 Rev. 1 Guide for Conducting Risk Assessments](https://csrc.nist.gov/publications/detail/sp/800-30/rev-1/final)
