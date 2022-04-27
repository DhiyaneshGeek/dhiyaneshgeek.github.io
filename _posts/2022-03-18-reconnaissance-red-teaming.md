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

[Domain Properties](https://attack.mitre.org/techniques/T1590/001/)
* Information about domains and their properties may include a variety of details, including what domain(s) the victim owns as well as administrative data **(ex: name, registrar, etc.)** and more directly actionable information such as contacts **(email addresses and phone numbers), business addresses, and name servers**.

The following tool can be used to enumerate domain properties

* [AADInternals](https://github.com/Gerenios/AADInternals) - AADInternals can gather information about a tenant’s domains using public Microsoft APIs.

```bash
# Get login information for a domain
Get-AADIntLoginInformation -Domain company.com
```

<p align="center">
  <img src="/images/reconnaissance/aadinternals.png">
</p>

[DNS](https://attack.mitre.org/techniques/T1590/002/)
* Adversaries gather information about the **victim's DNS** that can be used during targeting.
* DNS information may include a variety of details, including **registered name servers** as well as records that outline addressing for a target’s **subdomains, mail servers, and other hosts**.

The following tools and website allows the attacker to gather DNS information.

* [dig](https://toolbox.googleapps.com/apps/dig/) - dig is a network administration command-line tool for **querying** the Domain Name System.

```bash
dig google.com
```
<p align="center">
  <img src="/images/reconnaissance/dig1.png">
</p>

```bash
dig google.com -t mx +short #grab mail server information
```

<p align="center">
  <img src="/images/reconnaissance/dig2.png">
</p>

* [host](https://linux.die.net/man/1/host) - the host command is a **DNS lookup utility**, finding the **IP address** of a domain name.

```bash
host google.com
```

<p align="center">
  <img src="/images/reconnaissance/host.png">
</p>

* [dnsenum](https://github.com/fwaeytens/dnsenum) - dnsenum is a perl script that **enumerates DNS information**.

```bash
dnsenum --no-reverse google.com
```

<p align="center">
  <img src="/images/reconnaissance/dnsenum.png">
</p>

* [dns-brute-script](https://nmap.org/nsedoc/scripts/dns-brute.html) - Nmap will attempt to enumerate DNS hostnames by **brute forcing** popular subdomain names.
```bash
nmap -T4 -p 53 --script dns-brute google.com
```
<p align="center">
  <img src="/images/reconnaissance/nmap.png">
</p>

* [dnsrecon](https://github.com/darkoperator/dnsrecon) - Check all NS Records for **Zone Transfers**. Enumerate General DNS Records for a given Domain (MX, SOA, NS, A, AAAA, SPF and TXT). Perform common **SRV Record Enumeration**. Top Level Domain **(TLD)** Expansion.

```bash
dnsrecon -d google.com
```

<p align="center">
  <img src="/images/reconnaissance/dnsrecon.png">
</p>

* [dnsx](https://github.com/projectdiscovery/dnsx) - dnsx is a fast and **multi-purpose DNS toolkit allow to run multiple DNS queries** of your choice with a list of user-supplied resolvers.

<p align="center">
  <img src="/images/reconnaissance/dnsx.png">
</p>

[Network Trust Dependencies](https://attack.mitre.org/techniques/T1590/003/)
* Adversaries gather information about the victim's network **trust dependencies** that can be used during targeting.
* Information about network trusts may include a variety of details, including second or third-party organizations/domains 
**(ex: managed service providers, contractors, etc.)** that have connected (and potentially elevated) network access.

[Network Topology](https://attack.mitre.org/techniques/T1590/004/)
* Adversaries gather information about the victim's **network topology** that can be used during targeting.
* Information about network topologies may include a variety of details, including the **physical and/or logical arrangement** of both external-facing and internal network environments. 
* This information may also include specifics regarding network devices **(gateways, routers, etc.)** and other infrastructure.

[IP Addresses](https://attack.mitre.org/techniques/T1590/005/)
* Adversaries gather the **victim's IP** addresses that can be used during targeting.
* **Public IP addresses** may be allocated to organizations by block, or a range of **sequential addresses**.
* Information about **assigned IP addresses** may include a variety of details, such as which IP addresses are in use. 
* IP addresses may also enable an adversary to derive other details about a victim, such as **organizational size, physical location(s), Internet service provider**, and or where/how their **publicly-facing** infrastructure is hosted.

The following tools and website allows the attacker to gather IP Addresses.

* [NetblockTool](https://github.com/NetSPI/NetblockTool) - Find netblocks owned by a company

```bash
python3 NetblockTool.py -v Google
```
<p align="center">
  <img src="/images/reconnaissance/netblock.png">
</p>

* [Hurricane Electric BGP Toolkit](https://bgp.he.net/) - Hurricane Electric operates the largest **Internet Protocol version 4 (IPv4) and Internet Protocol version 6 (IPv6)** transit networks globally, as measured by the count of peering interconnections to other networks.

<p align="center">
  <img src="/images/reconnaissance/bgp.png">
</p>

* [SurfaceBrowser](https://securitytrails.com/app/sb) - Know the external Internet surface area of any company through a **simple web-based interface**. 

<p align="center">
  <img src="/images/reconnaissance/surface.png">
</p>

[Network Security Appliances](https://attack.mitre.org/techniques/T1590/006/)
* Adversaries gather information about the victim's **network security appliances** that can be used during targeting.
* Information about network security appliances may include a variety of details, such as the existence and specifics of deployed **firewalls, content filters, and proxies/bastion hosts**.
* Adversaries may also target information about victim network-based intrusion detection systems **(NIDS)** or other appliances related to **defensive cybersecurity operations**.

<strong>[Gather Victim Org Information](https://attack.mitre.org/techniques/T1591/)</strong>
* Adversaries gather information about the **victim's organization** that can be used during targeting. 
* Information about an organization may include a variety of details, including the names of **divisions/departments, specifics of business operations, as well as the roles and responsibilities of key employees**.


