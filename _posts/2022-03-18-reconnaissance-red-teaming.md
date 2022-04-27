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

<strong>[Gather Victim Identity Information](https://attack.mitre.org/techniques/T1589/)</strong>          
* Adversaries gather information about the **victim's identity** that can be used during targeting.
* Information about identities may include a variety of details, including personal data **(ex: employee names, email addresses, etc.)** as well as sensitive details such as **credentials.**

[Credentials](https://attack.mitre.org/techniques/T1589/001/)
* Adversaries gather **credentials** that can be used during targeting. 
* Account credentials gathered by adversaries may be those directly associated with the target **victim organization** or attempt to take advantage of the tendency for users to use the **same passwords across personal and business accounts**.

The following tools and website allows the attacker to gather leaked credentials

* [DeHashed](https://www.dehashed.com/) - DeHashed is described as the largest & fastest **data breach search engine**, its **API Key** can be used to integrate with other tools like [dehashQuery](https://github.com/grahamhelton/dehashQuery) to download breach results as shown below.

```bash
python3 dehashed.py -o -d domain.com -a API-KEY -u user@domain.com
```

<p align="center">
  <img src="/images/reconnaissance/dehased.gif">
</p>

* [We Leak Info](https://weleakinfo.to/) - Have your passwords been compromised? Find out by searching through over **12 billion records** and **10,000 data breaches.**

<p align="center">
  <img src="/images/reconnaissance/leakinfo.png">
</p>

* [IntelligenceX](https://intelx.io/) - Intelligence X is a search engine and data archive. Search **Tor, I2P, data leaks** and the public web by email, domain, IP, CIDR, Bitcoin address and more.

<p align="center">
  <img src="/images/reconnaissance/intelligencex.png">
</p>

* [Have I Been Pwned?](https://haveibeenpwned.com/) - Have I Been Pwned? is a website that allows Internet users to check whether their **personal data has been compromised by data breaches**.

<p align="center">
  <img src="/images/reconnaissance/haveibeenpwned.png">
</p>

[Email Addresses](https://attack.mitre.org/techniques/T1589/002/)
* Adversaries gather **email addresses** that can be used during targeting. Even if internal instances exist, organizations may have **public-facing** email infrastructure and addresses for employees.

The following tools and website allows the attacker to gather email addresses

* [Hunter.io](https://hunter.io/) - Hunter is the leading solution to find and verify professional email addresses.

<p align="center">
  <img src="/images/reconnaissance/hunterio.png">
</p>

* [EmailHarvester](https://github.com/maldevel/EmailHarvester) - A tool to retrieve Domain email addresses from **Search Engines**.

```bash
python3 EmailHarvester.py -d google.com -e all
```
<p align="center">
  <img src="/images/reconnaissance/emailharvester.png">
</p>

* [Infoga](https://github.com/m4ll0k/infoga) - Infoga is a tool gathering **email accounts** informations (ip,hostname,country,...) from different public source (search **engines, pgp key servers and shodan**).

```bash
python3 infoga.py --domain vmware.com --source all
```

<p align="center">
  <img src="/images/reconnaissance/infoga.png">
</p>

* [Skymen](https://www.skymem.info/) - Find email addresses of **companies** and people.

<p align="center">
  <img src="/images/reconnaissance/skymen.png">
</p>

[Employee Names](https://attack.mitre.org/techniques/T1589/003/)
* Adversaries gather **employee names** that can be used during targeting.
* Employee names be used to derive email addresses as well as to help guide other reconnaissance efforts and/or **craft more-believable lures**.

The following tools can be used to gather employee names

* [linkedin-employee-scraper](https://github.com/ChrisAD/linkedin-employee-scraper) - Extract all **employees** from **LinkedIn**. Especially useful for companies with thousands of pages and employees. Script is run as a userscript, running in e.g. Chromes **Tampermonkey** or Firefox's **Greasemonkey**.

<p align="center">
  <img src="/images/reconnaissance/tampermonkey.png">
</p>

<p align="center">
  <img src="/images/reconnaissance/linkedinscrape.png">
</p>

<strong>[Gather Victim Network Information](https://attack.mitre.org/techniques/T1590/)</strong>
* Adversaries gather information about the **victim's networks** that can be used during targeting.
* Information about **networks** may include a variety of details, including administrative data **(ex: IP ranges, domain names, etc.)** as well as specifics regarding its topology and operations.



