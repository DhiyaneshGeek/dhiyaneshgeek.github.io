---
layout: post
title: Reconnaissance
date: 2022-04-28
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
* The reconnaissance consists of techniques that involve gathering information related to the target actively as well as passively. The gathered information may include details of the **victim organisation, infrastructure or staff/personnel**.

<p align="center"><strong>ATT&CK Matrix for Enterprise</strong></p>

<p align="center">
  <img src="/images/reconnaissance/techniques.png">
</p>

<p align="center"><strong>Reconnaissance Tactics</strong></p>

<strong>[Gather Victim Identity Information](https://attack.mitre.org/techniques/T1589/)</strong>          
* Adversaries gather information about the **victim's identity** that can be used during targeting.
* Information about identities include a variety of details, including personal data **(ex: employee names, email addresses, etc.)** as well as sensitive details such as **credentials.**

[Credentials](https://attack.mitre.org/techniques/T1589/001/)
* Adversaries gather **credentials** that can be used during targeting. 
* Account credentials gathered by adversaries to be those directly associated with the target **victim organization** or attempt to take advantage of the tendency for users to use the **same passwords across personal and business accounts**.

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
* Adversaries gather **email addresses** that can be used during targeting even if internal instances exist, organizations have **public-facing** email infrastructure and addresses for employees.

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

* [Infoga](https://github.com/m4ll0k/infoga) - Infoga is a tool that gathering **email accounts** informations (ip,hostname,country,...) from different public source (search **engines, pgp key servers and shodan**).

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
* Employee names be used to determine email addresses as well as to help guide other reconnaissance efforts and/or **craft more-believable lures**.

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
* Information about **networks** include a variety of details, including administrative data **(ex: IP ranges, domain names, etc.)** as well as specifics regarding its topology and operations.

[Domain Properties](https://attack.mitre.org/techniques/T1590/001/)
* Information about domains and their properties include a variety of details, including what domain(s) the victim owns as well as administrative data **(ex: name, registrar, etc.)** and more directly actionable information such as contacts **(email addresses and phone numbers), business addresses, and name servers**.

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
* DNS information include a variety of details, including **registered name servers** as well as records that outline addressing for a target’s **subdomains, mail servers, and other hosts**.

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

[IP Addresses](https://attack.mitre.org/techniques/T1590/005/)
* Adversaries gather the **victim's IP** addresses that can be used during targeting.
* **Public IP addresses** to be allocated to organizations by block, or a range of **sequential addresses**.
* Information about **assigned IP addresses** include a variety of details, such as which IP addresses are in use. 
* IP addresses also enable an adversary to derive other details about a victim, such as **organizational size, physical location(s), Internet service provider**, and or where/how their **publicly-facing** infrastructure is hosted.

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

* [ipinfo.io](https://ipinfo.io/) - Comprehensive IP address data, IP **geolocation API**.

<p align="center">
  <img src="/images/reconnaissance/ipinfo.png">
</p>

<strong>[Search Open Technical Databases](https://attack.mitre.org/techniques/T1596/)</strong>
* Adversaries search freely available **technical databases** for information about victims that can be used during targeting
* Information about victims available in **online databases** and **repositories**, such as registrations of domains/certificates as well as public collections of network data/artifacts gathered from traffic and/or scans.

[WHOIS](https://attack.mitre.org/techniques/T1596/002/)
* Adversaries search public **WHOIS data** for information about victims that can be used during targeting
* WHOIS data is stored by **regional Internet registries (RIR)** responsible for allocating and assigning Internet resources such as domain names
Anyone can query WHOIS servers for information about a **registered domain**, such as assigned **IP blocks, contact information, and DNS nameservers**.

The following tools and website allows the attacker to gather whois information.

* [whois](https://github.com/weppos/whois) - whois is a widely used **Internet record listing** that contains the details of who owns a **domain name** and how to get in touch with them.

<p align="center">
  <img src="/images/reconnaissance/whois.png">
</p>

* [ICANN Lookup](https://lookup.icann.org/en/lookup) - The ICANN registration data lookup tool gives you the ability.

<p align="center">
  <img src="/images/reconnaissance/icann.png">
</p>

[Digital Certificates](https://attack.mitre.org/techniques/T1596/003/)
* Adversaries search **public digital certificate** data for information about victims that can be used during targeting.
* Digital certificates are issued by a **certificate authority (CA)** in order to cryptographically verify the origin of signed content.
* These certificates, such as those used for encrypted web traffic **(HTTPS SSL/TLS communications)**, contain information about the **registered organization** such as name and location.

The following website allows the attacker to gather digital certificate information.

* [crt.sh](https://crt.sh/) - crt.sh is a web interface to a distributed database called the **certificate transparency logs**.

<p align="center">
  <img src="/images/reconnaissance/crtsh.png">
</p>

<p align="center">
  <img src="/images/reconnaissance/crt.png">
</p>

[CDN](https://attack.mitre.org/techniques/T1596/004/)
* Adversaries search **content delivery network (CDN)** data about victims that can be used during targeting.
* CDNs allow an organization to host content from a distributed, **load balanced array of servers**.
* CDNs also allow organizations to customize content delivery based on the requestor’s **geographical region**.

The following tools allows the attacker to gather CDN information.

* [findcdn](https://github.com/cisagov/findcdn) - findCDN is a tool created to help accurately identify **what CDN a domain is using**.

```bash
findcdn list asu.edu -t 7 --double
```
<p align="center">
  <img src="/images/reconnaissance/findcdn.gif">
</p>

<strong>[Search Open Websites/Domains](https://attack.mitre.org/techniques/T1593/)</strong>
* Adversaries search **freely available websites and/or domains** for information about victims that can be used during targeting.
* Information about victims available in various **online sites**, such as social media, new sites, or those hosting information about business operations such as hiring or requested/rewarded contracts.

The following tools and website allows the attacker to gather subdomain information.

* [subfinder](https://github.com/projectdiscovery/subfinder) - Subfinder is a subdomain discovery tool that discovers **valid subdomains** for websites.

```
subfinder -d google.com -all -v
```
<p align="center">
  <img src="/images/reconnaissance/subfinder.png">
</p>

* [assetfinder](https://github.com/tomnomnom/assetfinder) - Find **domains** and **subdomains** related to a given domain.

```bash
assetfinder --subs-only google.com
```

<p align="center">
  <img src="/images/reconnaissance/assetfinder.png">
</p>

* [knockknock](https://github.com/harleo/knockknock) - A simple **reverse whois lookup tool** which returns a list of domains owned by people or companies.

```bash
knockknock -n google.com -p
```

<p align="center">
  <img src="/images/reconnaissance/knockknock.png">
</p>

* [findomain](https://github.com/Findomain/Findomain) - The complete solution for domain recognition. Supports screenshoting, port scan, HTTP check, data import from other tools, **subdomain monitoring**, alerts via Discord, Slack and Telegram, multiple API Keys for sources and much more.

```bash
findomain -t google.com
```
<p align="center">
  <img src="/images/reconnaissance/findomain.png">
</p>

* [hakrevdns](https://github.com/hakluke/hakrevdns) - Small, fast tool for performing **reverse DNS lookups en masse**.

```bash
prips 173.0.84.0/24 | hakrevdns -d
```
<p align="center">
  <img src="/images/reconnaissance/hakrevdns.png">
</p>

* [Amass](https://github.com/OWASP/Amass) - In-depth **Attack Surface Mapping** and Asset Discovery.

```bash
amass intel -org 'Sony Corporation of America'  #fetch ASN & CIDR IP Range of a Company
```

<p align="center">
  <img src="/images/reconnaissance/asn.png">
</p>

```bash
amass intel -active -asn 3725 -ip   #enumerate subdomains & IP Address from ASN
```

<p align="center">
  <img src="/images/reconnaissance/subip.png">
</p>

```bash
amass intel -active -asn 3725    #enumerate subdomains only from ASN
```

<p align="center">
  <img src="/images/reconnaissance/subsonly.png">
</p>

```bash
amass intel -active -cidr 160.33.96.0/23   #enumerate subdomains from cidr range
```

<p align="center">
  <img src="/images/reconnaissance/cidr.png">
</p>

```bash
amass intel -asn 3725 -whois -d sony.com   #enumerate subdomains using asn & whois
```

<p align="center">
  <img src="/images/reconnaissance/asnwhois.png">
</p>

```bash
amass enum -d sony.com -active -cidr 160.33.99.0/24,160.33.96.0/23 -asn 3725   #enumerate subdomains using cidr & asn
```

<p align="center">
  <img src="/images/reconnaissance/cidrasn.png">
</p>

* [Google Certificate transparency](https://github.com/rook1337/googlecertfarm) - this tools allows the user to gather **domains & subdomains from SSL Certificate**.

```bash
python3 googlecertfarm.py -d google.com
```
<p align="center">
  <img src="/images/reconnaissance/googlecertfarm.png">
</p>

<strong>[Active Scanning](https://attack.mitre.org/techniques/T1595/)</strong>
* Adversaries execute **active reconnaissance** scans to gather information that can be used during targeting.
* Active scans are those where the adversary probes **victim infrastructure via network traffic**, as opposed to other forms of reconnaissance that do not involve direct interaction.

[Scanning IP Blocks](https://attack.mitre.org/techniques/T1595/001/)
* Adversaries scan **victim IP blocks** to gather information that can be used during targeting.
* **Public IP addresses** may be allocated to organizations by block, or a range of **sequential addresses**.

The following tools allows the attacker to scan IP Blocks information.

* [mapcidr](https://github.com/projectdiscovery/mapcidr) - A utility program to perform multiple operations for a given subnet/cidr ranges.

```bash
mapcidr -cidr 173.0.84.0/24
```
<p align="center">
  <img src="/images/reconnaissance/mapcidr.png">
</p>

* [nmap](https://github.com/nmap/nmap) - Nmap is used to **discover hosts** and services on a computer network by sending packets and analyzing the responses.

```bash
nmap -v -A scanme.nmap.org  #basic scan for detection
```
<p align="center">
  <img src="/images/reconnaissance/nmap.png">
</p>

* [naabu](https://github.com/projectdiscovery/naabu) - A fast port scanner written in go with a focus on **reliability** and **simplicity**.

```bash
naabu -host 104.16.99.52
```
<p align="center">
  <img src="/images/reconnaissance/naabu-run.png">
</p>

* [massscan](https://github.com/robertdavidgraham/masscan) - TCP port scanner, spews **SYN packets asynchronously**, scanning entire Internet in under 5 minutes.

```bash
masscan 104.16.100.52 -p0-65535
```

<p align="center">
  <img src="/images/reconnaissance/massscan.png">
</p>

[Vulnerability Scanning](https://attack.mitre.org/techniques/T1595/002/)
* Adversaries **scan victims for vulnerabilities** that can be used during targeting
* Vulnerability scans typically check if the configuration of a target host/application **(ex: software and version)** potentially aligns with the target of a **specific exploit** the adversary may seek to use.

The following tools allows the attacker to perform vulnerability scanning.

* [nuclei](https://github.com/projectdiscovery/nuclei) - Fast and customizable vulnerability scanner based on simple YAML based DSL.

<p align="center">
  <img src="/images/reconnaissance/nuclei.gif">
</p>

* [reNgine](https://github.com/yogeshojha/rengine) - reNgine is a web application reconnaissance suite with focus on highly configurable streamlined recon process via Engines, recon data correlation, continuous monitoring, recon data backed by database and simple yet intuitive User Interface.

<p align="center">
  <img src="/images/reconnaissance/rengine.gif">
</p>

* [Osmedeus](https://github.com/j3ssie/osmedeus) - A Workflow Engine for **Offensive Security**.

<p align="center">
  <img src="/images/reconnaissance/osmedeus.png">
</p>

* [Sn1per Professional](https://github.com/1N3/Sn1per) - Discover the attack surface and prioritize risks with our **continuous** Attack Surface Management (ASM) platform.

<p align="center">
  <img src="/images/reconnaissance/sniperpro.png">
</p>

**Reference:**                
[Reconnaissance, Tactic TA0043 - Enterprise | MITRE ATT&CK](https://attack.mitre.org/tactics/TA0043/)                
[Red Team Reconnaissance Techniques](https://www.linode.com/docs/guides/red-team-reconnaissance-techniques/)                  
[Red Team Assessment Phases: Reconnaissance](https://resources.infosecinstitute.com/topic/red-team-assessment-phases-reconnaissance/)        
[Red Teaming Toolkit Reconnaissance](https://github.com/infosecn1nja/Red-Teaming-Toolkit#Reconnaissance)

<p align="center">
  <img src="https://media.giphy.com/media/RbDKaczqWovIugyJmW/giphy.gif">
</p>

Thanks a lot for reading !!!.
