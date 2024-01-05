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

```subfinder -d apple.com -all -v -rl 1 -t 1 -o apple-subs.txt```

<p align="center">
  <img src="/images/subfinder/terminal-1.png"> 
</p>

- I remember seeing more subdomain count for SecurityTrails in the Dashboard but it's showing only less results.

- Here in the below image, you can see `apple.com` contains around 39,904 subdomains.

<p align="center">
  <img src="/images/subfinder/securitytrail.png" width="800" height="600">
</p>

- Securitytrails (Free Version) API Configured with Subfinder. (Only 2000 results returned)

`subfinder -d apple.com -s securitytrails -rl 1 -t 1 -v`

<p align="center">
  <img src="/images/subfinder/free.png">
</p>

- It is also mentioned in the API documentation only 2000 results will be returned with Free API Key.

<p align="center">
  <img src="/images/subfinder/docs-1.png">
</p>

- Securitytrails (Professional Paid Version) API Configured with Subfinder. (Only 10000 results returned)

`subfinder -d apple.com -s securitytrails -rl 1 -t 1 -v`

<p align="center">
  <img src="/images/subfinder/paid.png">
</p>

- It is also mentioned in the API documentation only 10000 results will be returned with `Professional/Business` Plan API Key.

<p align="center">
  <img src="/images/subfinder/docs-2.png">
</p>

- To get all subdomains, we need to use the [Domain Search Endpoint](https://docs.securitytrails.com/reference/domain-search)https://docs.securitytrails.com/reference/domain-search with **Scroll ID** Enabled as shown in the above image.

- After going through the documentation found this POST request, where it discloses the **scroll_id**, **total_pages**, **record_count**.

```
POST /v1/domains/list?include_ips=false&scroll=true HTTP/2
Host: api.securitytrails.com
User-Agent: SonyEricssonW810i/R4EA Browser/NetFront/3.3 Profile/MIDP-2.0 Configuration/CLDC-1.1 UP.Link/6.3.0.0.0
Content-Length: 35
Accept: */*
Accept-Language: en
Apikey: YOUR_API_KEY_HERE
Connection: close
Content-Type: application/json
Accept-Encoding: gzip, deflate, br

{"query":"apex_domain='apple.com'"}
```

<p align="center">
  <img src="/images/subfinder/post-request.png">
</p>

**scroll_id -** Scroll ID is generated newly every time when the POST request is sent with requested domain or subdomain. (Not Constant & Has Time Limit)

**total_pages -** Per Page 100 hostnames are present by default. (ie: if the total page is 400 means 400*100 = 40000 will be present or lesser than that)

**record_count** - Shows what’s the exact total number of subdomains present.
