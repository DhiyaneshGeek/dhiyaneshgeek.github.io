---
layout: post
title: Subfinder Unleashed
date: 2024-01-03
summary: Maximizing Subdomain Discovery with SecurityTrails
categories: Research,Bug Bounty
tags: [Reconnaissance, Intelligent Automation, Bug Bounty]
---

Hi Everyone

I’m back with a blog post, sharing my experience about subdomain enumeration using [Subfinder](https://github.com/projectdiscovery/subfinder) with [SecurityTrails](https://securitytrails.com/app/account) Free/Paid API Keys.

<p align="center">
  <img src="/images/subfinder/subfinder-logo.png"> 
</p>

I was doing recon using Subfinder with API Keys configured in it and observed the following stats.

```
| Source          |    Duration   |    Results   |
|─────────────────|───────────────|──────────────|
| alienvault      |     1.386s    |     104      |
| anubis          |     356ms     |     17654    |
| bevigil         |     1.91s     |     158      |
| binaryedge      |     2.392s    |     21084    |
| censys          |     16.812s   |     2140     |
| certspotter     |     15.741s   |     2169     |
| chaos           |     879ms     |     35077    |
| commoncrawl     |     1.42s     |     13       |
| crtsh           |     2.166s    |     2609     |
| digitorus       |     881ms     |     100      |
| dnsdumpster     |     3.416s    |     112      |
| facebook        |     19.814s   |     8030     |
| fofa            |     3.19s     |     5236     |
| fullhunt        |     1.293s    |     2999     |
| github          |     9m57s     |     116      |
| hackertarget    |     1.777s    |     500      |
| leakix          |     2.327     |     10       |
| netlas          |     35.997s   |     200      |
| passivetotal    |     2.559s    |     4999     |
| quake           |     1.835s    |     227      |
| rapiddns        |     6.931s    |     8410     |
| redhuntlabs     |     1.21s     |     9963     |
| securitytrails  |     2.639s    |     2000     |
| shodan          |     264ms     |     676      |
| sitedossier     |     57s.730ms |     1000     |
| virustotal      |     5m.14s    |     2000     |
| waybackarchive  |     13.163s   |     10       |
| whoisxmlapi     |     3.56ms    |     10000    |
| zoomeyeapi      |     32.159s   |     9990     |
```
