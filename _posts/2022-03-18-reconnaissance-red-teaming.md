---
layout: post
title: Reconnaissance
date: 2022-03-26
summary: Red Teamer Perspective
categories: Red Teaming
tags: [Bugs, Write-Ups, Red Teaming]
---

Hi Everyone

I'm back with another blog on how Reconnaissance is carried out in a Red Teaming Engagement.

<p align="center">
  <img src="/images/reconnaissance/attack.png">
</p>

**What is Reconnaissance ?**      
* Reconnaissance consists of techniques that involve **adversaries actively** **or passively** gathering information that can be used to support targeting. Such information may include details of the **victim organization, infrastructure, or staff/personnel.**

<p align="center"><strong>ATT&CK Matrix for Enterprise</strong></p>

<p align="center">
  <img src="/images/reconnaissance/techniques.png">
</p>

<p align="center"><strong>Case Study of Reconnaissance</strong></p>

[Gather Victim Identity Information](https://attack.mitre.org/techniques/T1589/)          
* Adversaries gather information about the **victim's identity** that can be used during targeting.
* Information about identities may include a variety of details, including personal data **(ex: employee names, email addresses, etc.)** as well as sensitive details such as **credentials.**

[Credentials](https://attack.mitre.org/techniques/T1589/001/)
* Adversaries gather **credentials** that can be used during targeting. 
* Account credentials gathered by adversaries may be those directly associated with the target **victim organization** or attempt to take advantage of the tendency for users to use the **same passwords across personal and business accounts**.

The following tools and website allows the attacker to gather leaked credentials

1. [DeHashed](https://www.dehashed.com/) - DeHashed is described as the largest & fastest data breach search engine, its API Key can be used to integrate with other tools like [dehashQuery](https://github.com/grahamhelton/dehashQuery) to download breach results as shown below.

```bash
python3 dehashed.py -o -d domain.com -a API-KEY -u user@domain.com
```

<p align="center">
  <img src="/images/reconnaissance/dehased.gif">
</p>

2. [We Leak Info](https://weleakinfo.to/) - Have your passwords been compromised? Find out by searching through over 12 billion records and 10,000 data breaches.

<p align="center">
  <img src="/images/reconnaissance/leakinfo.png">
</p>

3. [IntelligenceX](https://intelx.io/) - Intelligence X is a search engine and data archive. Search Tor, I2P, data leaks and the public web by email, domain, IP, CIDR, Bitcoin address and more.

<p align="center">
  <img src="/images/reconnaissance/intelligencex.png">
</p>
