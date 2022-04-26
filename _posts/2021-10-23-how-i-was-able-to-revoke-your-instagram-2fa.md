---
layout: post
title: How I was able to revoke your Instagram 2FA
date: 2021-10-23
summary: Bypassing Rate Limit using IP Rotation
categories: Web Security
tags: [Bugs, Write-Ups, Web Security]
---

Hello Everyone,

Back in November 2019, I was going through one of the Old Writeups on Facebook Bug Bounty Reports, [_"How I was able to remove your Instagram Phone number"_](https://infosecwriteups.com/how-i-was-able-to-remove-your-instagram-phone-number-d346515e79c3) by [Neeraj Sonaniya](https://www.linkedin.com/in/neerajsonaniya/)

After reading this blog, it clicked in my mind that this can be bypassed by using **IP Rotation** Technique.

<p align="center">
  <img src="/images/instagram/ig1.jpeg">
</p>

**What is IP Rotation ?**

* IP rotation is the process of changing IP Address on Each Request that is sent to the server where assigned IP addresses are distributed to a device at random or at scheduled intervals.
* For example, when a connection is active via an Internet Service Provider (ISP), an IP address is automatically attached from a pool of IPs.

There is already a detailed article written [How to Rotate IP ADDRESS For Each Request in Burp Suite](https://lokeshdlk77.medium.com/how-to-rotate-ip-address-for-each-request-in-burp-suite-4e29645ef23e) by [Lokesh Kumar](https://www.facebook.com/lokeshdlk77)

<p align="center">
  <strong>Attack Scenario</strong>
</p>

<p align="center">
  <img src="/images/instagram/ig6.png">
</p>

**Steps to Reproduce:**

* Navigate to this URL https://www.instagram.com/, fill the Signup page.

* Enter the already Existing Instagram Users Mobile Number on the Signup page and Click on submit.

* It will navigate you to the below page.

<p align="center">
  <img src="/images/instagram/ig2.png">
</p>

* Enter any Random 6 digit OTP number, as shown below.

<p align="center">
  <img src="/images/instagram/ig3.png">
</p>

* Intercept the Request using Client Side Proxy such as Burpsuite.

<p align="center">
  <img src="/images/instagram/ig4.png">
</p>

* Now send the request to the **Intruder tab** in the Burpsuite and insert **$$** placeholder in the **sms_code** as shown below.

<p align="center">
  <img src="/images/instagram/ig5.png">
</p>

* Move to the **Payloads Option** tab and specify the payload option as below.

<p align="center">
  <img src="/images/instagram/ig7.png">
</p>

**NOTE:** 

Burpsuite User Option Configuration.

<p align="center">
  <img src="/images/instagram/ig8.png">
</p>

Open Proxy Configuration

<p align="center">
  <img src="/images/instagram/ig9.png">
</p>

* Click on the Attack button to start the attack.

* Now in the length you can see **2547** shows the code is **Invalid** as shown below.

<p align="center">
  <img src="/images/instagram/ig10.png">
</p>

* When the **Correct** code matches , it shows **3453** and **Account Created** as shown below.

<p align="center">
  <img src="/images/instagram/ig11.png">
</p>

* In result to this reaction, the number which belongs to the user gets an email that **"Phone number removed as Two Factor Authentication Method"**.

<p align="center">
  <img src="/images/instagram/ig14.png">
</p>

**Impact:-**

* The mobile number that is registered in Instagram can be reused to register again and it will take us to 6 digit OTP page which can be bypassed using IP Rotation Technique, this could be used to remove a confirmed mobile number from another user.

* If the User uses the same mobile number for Two factor authentication, it will disable the Two Factor authentication without that user's interaction.

**Timeline:-**

27 January 2020 at 01:40   - Report Submitted    
31 January 2020 at 11:00   - Not able to Reproduce     
6 February 2020 at 23:08   - Detailed POC Video Sent    
7 February 2020 at 01:01   - Issue Triaged    
14 February 2020 at 22:12  - Issue Patched    
14 February 2020 at 22:23  - **$5000** Bounty Awarded

<p align="center">
  <img src="/images/instagram/ig13.png">
</p>

**References:-** 

* [Bypassing IP Based Blocking with AWS API Gateway](https://rhinosecuritylabs.com/aws/bypassing-ip-based-blocking-aws/)   
* [How I Could Have Hacked Any Instagram Account](https://thezerohack.com/hack-any-instagram)   
* [How I Hacked Instagram Again](https://thezerohack.com/hack-instagram-again)    
* [Disable Any Unconfirmed Account in Facebook](https://lokeshdlk77.medium.com/disable-any-unconfirmed-account-in-facebook-123aeba19426)    
* [Confirming any new Email Address bug in Facebook (Part-4)](https://lokeshdlk77.medium.com/confirming-any-new-email-address-bug-in-facebook-part-4-70cfe1b4dca5)    

<p align="center">
  <img src="https://media.giphy.com/media/MBIcJIaE8SLY6jKrqp/giphy.gif">
</p>

This post is only for **Educational Purposes**.

Many websites uses **IP based Blocking** so please do not use this technique for **Illegal Activities**.

Thanks a lot for reading !!!.
