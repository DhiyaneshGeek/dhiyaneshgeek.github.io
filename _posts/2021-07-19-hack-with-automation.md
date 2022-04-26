---
layout: post
title: Hack with Automation !!!
date: 2021-07-19
summary: Security Automation, (re) defined
categories: Web Security
tags: [Bugs, Write-Ups, Web Security]
---

Hi Everyone   
In this blog, I will cover the process of automating and identifying the bugs using [Nuclei](https://github.com/projectdiscovery/nuclei) and the methodology of writing the customized [nuclei templates](https://github.com/projectdiscovery/nuclei-templates)

<p align="center">
  <img src="/images/recon/logo.png">
</p>

<p align="center">
  <strong>Nuclei</strong>
</p>

<p align="center">
  <img src="https://raw.githubusercontent.com/projectdiscovery/nuclei/master/static/nuclei-logo.png">
</p>

<p align="center">
  <img src="/images/hackwithautomation/nuclei.png">
</p>

* [Download](https://github.com/projectdiscovery/nuclei) and install nuclei using the following command

`GO111MODULE=on go get -v github.com/projectdiscovery/nuclei/v2/cmd/nuclei`

Note: Make sure that you have already installed the Go version of 1.14 or above in your system.

<p align="center">
  <strong>How it Works</strong>
</p>

<p align="center">
  <img src="/images/hackwithautomation/flow.png">
</p>

<p align="center">
  <strong>How to Write your Own Nuclei Template</strong>
</p>

Note : these below mentioned details are needed to write a good nuclei template

* Request
* Response
* Strict Matcher

<p align="center">
Let's take an example , you are reading about a bug or vulnerability blog as shown below.
</p>

<p align="center">
  <img src="/images/hackwithautomation/docker.png">
</p>

<p align="center">
Now we have the request , response and matcher to create a nuclei template.
</p>

<p align="center">
  <img src="/images/hackwithautomation/docker2.png">
</p>

<p align="center">
  <img src="/images/hackwithautomation/temp2.png">
</p>

* Scanning for misconfiguration on given list of URLs.

`nuclei -l target_urls.txt -t misconfigured-docker.yaml`

<p align="center">
  <img src="/images/hackwithautomation/dockermis.png">
</p>

Now let's take a another example, you are scrolling through **Twitter** and found some **New Exploit** has been released as shown below.


<p align="center">
  <img src="/images/hackwithautomation/twitter11.png">
</p>

If you know what is the **Request, Response, Strict Matcher** , it is easy to write a nuclei template.

<p align="center">
  <img src="/images/hackwithautomation/twitter2.png">
</p>

<p align="center">
  <img src="/images/hackwithautomation/temp1.png">
</p>

* Scanning for CVE-2020-36289 on given list of URLs.

`nuclei -l target_urls.txt -t CVE-2020-36289.yaml`

<p align="center">
  <img src="/images/hackwithautomation/jira.png">
</p>

More **Nuclei Templates** can be found here <https://github.com/projectdiscovery/nuclei-templates>.

**Note:-** 

Below mentioned are some of the places where you can find source for writing nuclei templates.

  * [Google Dorks](https://www.exploit-db.com/google-hacking-database) - GHDB is an index of search queries to get filtered search results
  * [Vulhub](https://github.com/vulhub/vulhub) - Pre-Built Vulnerable Environments Based on Docker-Compose
  * [PeiQi-WIKI-POC](https://github.com/PeiQi0/PeiQi-WIKI-POC) - Place where Exploits of various Tech Stack are Stored
  * [Awesome-CVE-POC](https://github.com/qazbnm456/awesome-cve-poc) - Collection about Proof of Concepts of Common Vulnerabilities and Exposures


Multiple search engines are available for the information gathering of various technologies that are exposed on the Internet. These can prove to be useful while creating the nuclei templates. Some of which are as follows : -
  * [Shodan](https://www.shodan.io/) - Search Engine for the Internet of Everything
  * [Fofa](https://fofa.so/) - Cyberspace Surveying and Mapping
  * [PublicWWW](https://publicwww.com/) - Source Code Search Engine
  * [ZoomEye](https://www.zoomeye.org/) - Cyberspace Search Engine
  * [Spyse](https://spyse.com/) - Internet Assets Search Engine


<p align="center">
  <strong>Nuclei Unleashed</strong>
</p>

<p align="center">Nuclei is capable of doing the following things</p>

* Out-Of-Band Interaction (OOB) using [Interactsh](https://github.com/projectdiscovery/interactsh)

<p align="center">
  <img src="/images/hackwithautomation/oob-test.gif">
</p>

* [File Requests](https://blog.projectdiscovery.io/secret-token-scanning-with-nuclei/) with Nuclei

<p align="center">
  <img src="/images/hackwithautomation/local-file-scan.gif">
</p>

* [Headless Requests](https://nuclei.projectdiscovery.io/templating-guide/protocols/headless/)

<p align="center">
  <img src="/images/hackwithautomation/headless.gif">
</p>

* [TCP Requests](https://nuclei.projectdiscovery.io/templating-guide/protocols/network/)

<p align="center">
  <img src="/images/hackwithautomation/tcp.gif">
</p>

* [CI/CD Pipeline Integration](https://blog.projectdiscovery.io/nuclei-v2-3-0-release/)

<p align="center">
  <img src="/images/hackwithautomation/integration.gif">
</p>

* [Tag Based Execution Support](https://nuclei.projectdiscovery.io/)

<p align="center">
  <img src="/images/hackwithautomation/tag.gif">
</p>

* Author Based Execution

<p align="center">
  <img src="/images/hackwithautomation/author.png">
</p>

**Reference:-**      
[Writing Network Templates with Nuclei](https://blog.projectdiscovery.io/writing-network-templates-with-nuclei/)    
[Writing nuclei templates for WordPress CVEs](https://blog.projectdiscovery.io/writing-nuclei-templates-for-wordpress-cves/)    
[Writing security templates for Apache Airflow](https://blog.projectdiscovery.io/writing-security-templates-for-apache-airflow/)    
[Nuclei Unleashed - Quickly write complex exploits](https://blog.projectdiscovery.io/nuclei-unleashed-quickly-write-complex-exploits/)    
[Nuclei - Fuzz all the things](https://blog.projectdiscovery.io/nuclei-fuzz-all-the-things/)    
[Exploiting Race conditions with Nuclei](https://blog.projectdiscovery.io/exploiting-race-conditons/)    

Huge Shout out to **[Project Discovery Team](https://projectdiscovery.io/)** for creating Amazing Tools

<p align="center">
  <img src="https://media.giphy.com/media/LcfBYS8BKhCvK/giphy.gif">
</p>

<p align="center">
  Kudos to all the active contributors out there !!!
</p>

<p align="center">
  <img src="/images/hackwithautomation/contri.png">
</p>

<p align="center">
Thanks a lot for reading !!!.
</p>
