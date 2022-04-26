---
layout: post
title: Vishing
date: 2022-03-18
summary: Social Engineering Tactics to Convince Victims
categories: Red Teaming
tags: [Bugs, Write-Ups, Red Teaming]
---

Hi Everyone

I'm back with another blog on how Vishing Campaign is carried out in a Red Teaming Engagement.

<p align="center">
  <img src="/images/vishing/vishing-new-logo.png">
</p>

**What is Vishing ?**

* Telephone phishing, also known as vishing, is similar to email phishing performed over a telephonic call to gain information from the target victim.
* It involves the exploitation of a potential victim over the phone by **Impersonating** as another **entity/person/company** and eventually convince the person to share **Sensitive Information**.

<p align="center"><strong>Flow Chart of Vishing</strong></p>
<p align="center">
  <img src="/images/vishing/flow-chart.png">
</p>

**Tools and Apps used for Vishing:**

* [Dragnet](https://github.com/tevora-threat/Dragnet)
* SIP Account with [Zoiper](https://www.zoiper.com/)
* [Asterisk](https://www.asterisk.org/)
* [SpoofCard](http://www.spoofcard.com/)

**Caller ID Spoofing:**
* The Basic idea behind Caller ID spoofing is to change the information that is displayed on the caller ID display.
* Most spoofing is done using a VoIP (Voice over Internet Protocol) service or IP phone that uses VoIP to transmit calls over the internet.

**Useful Situations for Caller ID Spoofing:**

These can be used in a vishing campaign to display that a call is coming from:

1. A remote office
2. Inside the office
3. With partner organization
4. From Co-worker
5. A superior
6. Delivery company

**SIP & Virtual Number Providers**

* [CallWithUs](https://www.callwithus.com/) - Calls to your DID (phone number) will be redirected to your SIP phone, regular PSTN or cellular phone. You don't need a VoIP client to have calls to your DID redirected to your PSTN or mobile phone number.

<p align="center">
  <img src="/images/vishing/callwithus.png">
</p>

* [KeepCalling](https://keepcalling.com/) - It is a Virtual Number provider, allows the user to buy credit and make International phone calls to different countries.

<p align="center">
  <img src="/images/vishing/keepcalling1.png">
</p>

<p align="center">
  <img src="/images/vishing/keepcalling2.png">
</p>

<p align="center"><strong>Configuring SIP Account with Zoiper</strong></p>

* After successful purchase of SIP Account, download Zoiper [Android](https://play.google.com/store/apps/details?id=com.zoiper.android.app&hl=en_IN&gl=US) or [IOS](https://apps.apple.com/us/app/zoiper-lite-voip-soft-phone/id438949960).

* Navigate to **Settings** --> **Accounts** --> **+** --> **Yes** --> **Manual Configuration** --> **SIP account**.

<p align="center">
  <img src="/images/vishing/zoiper.png">
</p>

* Registration Status **OK** shows that the configuration is successful.
<p align="center"><strong>Demonstration of Vishing Scenario</strong></p>

{%include youtubePlayer.html id="BEHl2lAuWCk"%}

**Prevention for Vishing:**

* Remain Vigilant and Pay Attention during Phone calls.
* Verify the identity of those who ask for your information in person or over the phone before you release any information.
* Be Suspicious of Unrecognized Phone Numbers.

**References:**     
[Attack Vector - Vishing](https://www.social-engineer.org/framework/attack-vectors/vishing/)  
[Caller ID Spoofing](https://www.social-engineer.org/framework/se-tools/phone/caller-id-spoofing/)      
[Caller ID Spoofing: How to do it :)](https://medium.com/@Vicky_2619/caller-id-spoofing-how-to-do-it-2f34284c435e)      
[Tutorial - How to Setup Asterisk Caller ID Spoofing (to troll scammers)](https://www.youtube.com/watch?v=DZ0czppbamo)     
[Dragnet: Your Social Engineering Sidekick](https://www.youtube.com/watch?v=v0YZFJpmnxM)

<p align="center">
  <img src="https://media.giphy.com/media/l2JegkncG3OWpjlks/giphy.gif">
</p>

This post is only for **Educational Purposes**.

NOTE- “**Do not perform vishing** on the target company that you **do not have permission** for.”

Thanks a lot for reading !!!.
