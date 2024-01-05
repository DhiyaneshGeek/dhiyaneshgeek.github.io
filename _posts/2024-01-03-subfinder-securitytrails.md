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
  <img src="/images/subfinder/securitytrail.png" width="750">
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

- To get all subdomains, we need to use the [Domain Search Endpoint](https://docs.securitytrails.com/reference/domain-search) with **Scroll ID** Enabled as shown in the above image.

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

- **scroll_id -** Scroll ID is generated newly every time when the POST request is sent with requested domain or subdomain. (Not Constant & Has Time Limit)

- **total_pages -** Per Page 100 hostnames are present by default. (ie: if the total page is 400 means 400*100 = 40000 will be present or lesser than that)

- **record_count** - Shows what’s the exact total number of subdomains present.

- Now we need to extract the `scroll_id` and `re-use` again in the next request to move to the next page.

```
GET https://api.securitytrails.com/v1/scroll/8755a518ef6ad0dd71f6b89035d0de2f HTTP/1.1
Host: api.securitytrails.com
User-Agent: curl/7.84.0
Apikey: YOUR_API_KEY_HERE
Content-Type: application/json

```

<p align="center">
  <img src="/images/subfinder/scroll-id-req.png">
</p>

After figuring out the solution, initally wrote a Nuclei Template for Subdomain Enumeration 😎

```yaml
id: securitytrails-subdomain

info:
  name: SecurityTrail Subdomain Enum
  author: DhiyaneshDK,vinothkumar
  severity: unknown

self-contained: true
http:
  - raw:
      - |
        @once
        POST https://api.securitytrails.com/v1/domains/list?include_ips=false&scroll=true HTTP/1.1
        Host: api.securitytrails.com
        User-Agent: curl/7.84.0
        Accept: */*
        Apikey: {{api_key}}
        Content-Type: application/json

        {  "query": "apex_domain = '{{domain}}'"}

      - |
        GET https://api.securitytrails.com/v1/scroll/{{scroll_id}}?nuclei={{number}} HTTP/1.1
        Host: api.securitytrails.com
        User-Agent: curl/7.84.0
        Apikey: {{api_key}}
        Accept: application/json

    payloads:
      number: numbers.txt

    matchers:
      - type: dsl
        dsl:
          - 'contains(body_1,"scroll_id")'
          - 'status_code_2 == 200'
        condition: and

    extractors:
      - type: regex
        internal: true
        part: body_1
        name: scroll_id
        group: 1
        regex:
          - '"scroll_id": "([0-9a-z]+)"'

      - type: json
        part: body_2
        json:
          - '.["records"] | .[] | .["hostname"]'
        to: "subdomains.txt"
```

**First Request:** 

- **@once** - the 1st request will be sent _once_ during in the template execution, which means that it will **extract** the **scroll_id** and re-use in the second request again and again **without** generating a new scroll_id.

**Second Request:**

- **scroll_id** - Extracted Scroll ID from the first request will be **re-use** in the second request.

- **?nuclei=** - This is just a **dummy parameter** to bypass, move to the next page and view the results.

- **number** - We need to create a file name called **numbers.txt** to supply in the template (page number is referred here, suppose if the **total_pages** is 400 the numbers.txt file should contains numbering from 1 to 400 line by line).

**Extractors:**

- <strong>"scroll_id": "([0-9a-z]+)"</strong> - Regex to extract **scroll_id**

- <strong>.["records"] | .[] | .["hostname"]</strong> - JSON regex to display **hostnames** in CLI.

- <strong>to: "subdomains.txt"</strong> - Saves the **output** in subdomains.txt file.

```nuclei -t securitytrails-subdomain.yaml -var api_key=YOUR_API_KEY_HERE -var domain=apple.com -vv -rl 1```

<p align="center">
  <img src="/images/subfinder/term-2.png" width="750">
</p>

<p align="center">
  <img src="/images/subfinder/sub-count.png" width="750">
</p>

<p align="center">
  <img src="/images/subfinder/sb.png" width="750">
</p>

Now we are able to get all the subdomains from SecurityTrails, the above results matchs with the Dashboard Results as well 😄 !

- Later i raised a [Issue](https://github.com/projectdiscovery/subfinder/issues/1104) in Subfinder Repository to Support Pagination in SecurityTrails Paid API Keys.

<p align="center">
  <img src="/images/subfinder/issue.png" width="750">
</p>

- Same Day, the issue was fixed by [dogancanbakir](https://github.com/dogancanbakir) and [PR](https://github.com/projectdiscovery/subfinder/pull/1105) was raised 🔥

<p align="center">
  <img src="/images/subfinder/pr.png" width="750">
</p>

- I went ahead and tested out the PR using the following command.

```go install -v github.com/projectdiscovery/subfinder/v2/cmd/subfinder@introduce_pagination_to_st```

```subfinder -d apple.com -s securitytrails -rl 1 -t 1 -v -max-time 20```

- And **it Worked** and matched with the Dashboard results as shown below.

<p align="center">
  <img src="/images/subfinder/match.png" width="750">
</p>

**Note:**
- If the subdomains are more than **30k** while enumerating with SecurityTrails Paid Version of API key, users need to supply `-max-time` to increase the time limit of enumeration, since the minutes to wait for enumeration results `(default 10)`.
- Only the **Paid** Version of SecurityTrails returns the **Scroll ID**.
- Subfinder Supports both Free & Paid version, Free means 2000 results limited / Paid means full results (depends on **API quota left** in the account).

Thanks to [ᴠɪɴᴏᴛʜ ᴋᴜᴍᴀʀ](https://twitter.com/vinnyvinoth242), for sharing the trick to supply dummy parameter with value in second request in the Nuclei Template and move to next page with numbers 😃

Shoutout to [Dogan Can Bakir](https://github.com/dogancanbakir), for fixing the Pagination issue SecurityTrails API on Subfinder and enhancing further to support both Free/Paid Version along with rate limit 💯

**Reference:**
- [Subfinder: Fast passive subdomain enumeration tool](https://github.com/projectdiscovery/subfinder)
- [Security Trails Subdomain Enumeration Docs](https://docs.securitytrails.com/reference/domain-subdomains)
- [Recon with Me !!!](https://dhiyaneshgeek.github.io/bug/bounty/2020/02/06/recon-with-me/)

<p align="center">
  <img src="/images/subfinder/last.png" height="300" width="450">
</p>

Thanks a lot for reading !!!.
