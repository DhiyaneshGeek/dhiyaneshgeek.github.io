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
