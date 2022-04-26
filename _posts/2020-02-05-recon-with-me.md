---
layout: post
title: Recon with Me !!!
date: 2020-02-06
summary: Security Through Intelligent Automation
categories: Bug Bounty
tags: [Reconnaissance, Intelligent Automation, Bug Bounty]
---

Hi Everyone 

In this blog , i will cover automating the enumeration part of reconnaissance and finding bugs using it with the following set of tools.

  * [Subfinder](https://github.com/projectdiscovery/subfinder)
  * [Chaos](https://github.com/projectdiscovery/chaos-client)
  * [Nuclei](https://github.com/projectdiscovery/nuclei)
  * [Httpx](https://github.com/projectdiscovery/httpx)
  * [Notify](https://github.com/projectdiscovery/notify)
  * [Anew](https://github.com/tomnomnom/anew)
  
<p align="center">
  <img src="/images/recon/logo.png">
</p>

<p align="center">
  <strong>Subfinder</strong>
</p>

<p align="center">
  <img src="/images/recon/sub1.png">
</p>

<p align="center">
  <img src="/images/recon/description1.png">
</p>

* [Download](https://github.com/projectdiscovery/subfinder) and install subfinder using the following command

`GO111MODULE=on go get -v github.com/projectdiscovery/subfinder/v2/cmd/subfinder`

Note: Make sure go1.14+ is installed in your system.

* To run the tool on any of the Target website , use the following command

`subfinder -d google.com`

<p align="center">
  <img src="/images/recon/sub2.png">
</p>

* It was able to fetch more than 50246+ subdomains in a short amount of time.

* Then i noticed that we can use `-v` flag to see the verbose output of the sources that are used to enumerate the subdomains.

`subfinder -d google.com -v`

<p align="center">
  <img src="/images/recon/sub3.png">
</p>

* Sources like chaos,threatminer,sublist3r,alienvault ,etc,. are being used to enumerate the subdomains of the target. 

* In the Subfinder Github Repository it was mentioned that some of the services will not work until you set it up.

<p align="center">
  <img src="/images/recon/sub4.png">
</p>

* So i started looking into it to set-up the **config-file** with the API Keys that are mentioned to see what is the **major difference in the results of subdomain**

* Navigate to the following directory

`cd .config/subfinder/`

<p align="center">
  <img src="/images/recon/sub5.png">
</p>

`cat config.yaml` to see the config file

<p align="center">
  <img src="/images/recon/sub6.png">
</p>

* We can see many of the API Key services are **Empty** , so now are going to fill the necessary API Keys as source for Subdomain Enumeration.

**Note** The below following API Keys are **Free Of Cost** and has a Limited number of request in it.

  * binaryedge
  * censys
  * certspotter
  * chaos
  * dnsdb
  * github
  * intelx
  * passivetotal
  * robtex
  * securitytrails
  * shodan
  * spyse
  * urlscan
  * virustotal
  * zoomeye

**Binaryedge** 

**1 :** [Sign up](https://app.binaryedge.io/sign-up) for a free account, verify the account.

**2 :** Login into the account and Navigate to this URL <https://app.binaryedge.io/account/api> and give a name to the **TOKEN** and Click on Generate Token.

<p align="center">
  <img src="/images/recon/sub7.png">
</p>

**Censys**

**1 :** [Sign up](https://censys.io/register) for a free account, verify the account.

**2 :** Login into the account and Navigate to this URL <https://censys.io/account/api>  and you will be able to get **API ID** and **Secret**

<p align="center">
  <img src="/images/recon/sub8.png">
</p>

**Certspotter** 

**1 :** [Sign up](https://sslmate.com/signup?for=certspotter_api) for a free account.

**2 :** Login into the account and Navigate to this URL <https://sslmate.com/account/api_credentials> and you will be able to get the **API Key**

Note : 100 queries an hour is **free**.

<p align="center">
  <img src="/images/recon/sub9.png">
</p>

**Chaos**

**1 :** Navigate to this URL <https://chaos.projectdiscovery.io/#/> 

**2 :** Early access is provided basis on signup and queue and Invite are send out Weekly basis.

**3 :** Contributor access is Provided on the basis of **PR** that is done under `github.com/projectdiscovery/*`.

<p align="center">
  <img src="/images/recon/sub10.png">
</p>

**DNSdb**

**1 :** [Sign up](https://www.farsightsecurity.com/dnsdb-community-edition/) for a free community account.

**2 :** It will ask for Company Email , use [Temp Email](https://temp-mail.org/).

**3 :** Create an account and verify the email and get the **API Key**. 

Note : It has 30-day renewal (with valid email confirmation)

<p align="center">
  <img src="/images/recon/sub11.png">
</p>

**Github** 

**1 :** [Sign up](https://github.com/join) for a free account, verify the account.

**2 :** Navigate to this URL <https://github.com/settings/tokens> and generate a **Personal access tokens**.

<p align="center">
  <img src="/images/recon/sub12.png">
</p>

**Intelx**

**1 :** [Sign up](https://intelx.io/signup) for a free account, verify the account.

**2 :** Navigate to this URL <https://intelx.io/account?tab=developer> and you will get the **API details**.

Note: Trial 1 week for Free

<p align="center">
  <img src="/images/recon/sub13.png">
</p>

**Passivetotal** 

**1 :** [Sign up](https://community.riskiq.com/home) for a free account, verify the account.

**2 :** Login into the account and Navigate to this URL <https://community.riskiq.com/settings>  and you will be able to get **KEY** and **Secret** .

<p align="center">
  <img src="/images/recon/sub14.png">
</p>

**Robtex**

**1 :** Sign in using the google **Gmail Account**

**2 :** Navigate to this URL <https://www.robtex.com/dashboard/> , you will get the **API-Key** details.

<p align="center">
  <img src="/images/recon/sub15.png">
</p>

**Security Trails** 

**1 :** [Sign up](https://securitytrails.com/app/signup) for a free account, verify the account.

**2 :** Login into the account and Navigate to this URL <https://securitytrails.com/app/account/credentials>  and you will be able to get **API Key** .

Note : Monthly Quoto is 50 API Requests.

<p align="center">
  <img src="/images/recon/sub16.png">
</p>

**Shodan**

**1 :** [Register](https://account.shodan.io/login) for a shodan account.

**2 :** Login into the account and navigate to this URL <https://account.shodan.io/> , you will get the **API Key** details.

<p align="center">
  <img src="/images/recon/sub17.png">
</p>

**Spyse**

**1 :** [Register](https://spyse.com/user/registration) for a Spyse account and verify it.

**2 :** Login into the account and navigate to this URL <https://spyse.com/user> , you will get the **API Token** details.

Note : It has 100 API Token valid for 5 days during the Trail Period.

<p align="center">
  <img src="/images/recon/sub18.png">
</p>

**UrlScan** 

**1 :** [Sign up](https://urlscan.io/user/signup) for a free account, verify the account.

**2 :** Login into the account and Navigate to this URL <https://urlscan.io/user/profile/>  and click on Create new **API Key**.

<p align="center">
  <img src="/images/recon/sub19.png">
</p>

**Virustotal**

**1 :** [Register](https://www.virustotal.com/gui/join-us) for a Virustotal account and verify it.

**2 :** Login into the account and navigate to this URL <https://www.virustotal.com/gui/user/username/apikey> , you will get the **API Key** details.

<p align="center">
  <img src="/images/recon/sub20.png">
</p>

**Zoom Eye**

**1 :** [Register](https://sso.telnet404.com/accounts/register/) for a ZoomEye account and verify it.

**2 :** Login into the account and navigate to this URL <https://www.zoomeye.org/profile> , you will get the **API Key** details.

<p align="center">
  <img src="/images/recon/sub21.png">
</p>

<p align="center">
  <strong>Now the Final Config File Looks Full!!!</strong>
</p>

<p align="center">
  <img src="/images/recon/sub22.png">
</p>

<p align="center">Now Let us compare the Results <strong>Before</strong> and <strong>After</strong> Adding API Keys.</p>

<p align="center">
  <strong>Before API Key</strong>
</p>

<p align="center">
  <img src="/images/recon/sub24.png">
</p>

<p align="center">
  <strong>After API Key</strong>
</p>

<p align="center">
  <img src="/images/recon/sub23.png">
</p>

* From this we can see we got **114817** Addtional subdomains after using **Free** API Keys.

* Only difference is the duration that is taken during the API scan is **slow** than the normal one.

<p align="center">
  <strong>Httpx</strong>
</p>

<p align="center">
  <img src="/images/recon/httpx1.png">
</p>

<p align="center">
  <img src="/images/recon/description2.png">
</p>

* [Download](https://github.com/projectdiscovery/httpx) and install httpx using the following command

`GO111MODULE=on go get -v github.com/projectdiscovery/httpx/cmd/httpx`

Note: Make sure go1.14+ is installed in your system.

* To run this tool against all the hosts and subdomains in hosts.txt and returns URLs running HTTP webserver, use the following command

`cat hosts.txt | httpx`

<p align="center">
  <img src="/images/recon/httpx2.png">
</p>

<p align="center">
  <strong>Nuclei</strong>
</p>

<p align="center">
  <img src="/images/recon/nuclei1.png">
</p>

<p align="center">
  <img src="/images/recon/description3.png">
</p>

* [Download](https://github.com/projectdiscovery/nuclei) and install nuclei using the following command

`GO111MODULE=on go get -v github.com/projectdiscovery/nuclei/v2/cmd/nuclei`

Note: Make sure go1.14+ is installed in your system.

* Scanning for misconfiguration on given list of URLs.

`nuclei -l target_urls.txt -t misconfigured-docker.yaml`

<p align="center">
  <img src="/images/recon/nuclei2.png">
</p>

More **Nuclei Templates** can be found here <https://github.com/projectdiscovery/nuclei-templates>.

<p align="center">
  <strong>Notify</strong>
</p>

<p align="center">
  <img src="/images/recon/notify1.png">
</p>

<p align="center">
  <img src="/images/recon/description4.png">
</p>

* [Download](https://github.com/projectdiscovery/notify) and install notify using the following command

`GO111MODULE=on go get -v github.com/projectdiscovery/notify/cmd/notify`

Note: Make sure go1.14+ is installed in your system.

* Notify also supports piping output of any tool and send it over discord/slack channel as notification.

* Now we need to configure **Slack Webhook URL** under the `notify` config file.

* Navigate to the following directory.

`cd .config/notify/`

cat `notify.conf` to see the config file.

<p align="center">
  <img src="/images/recon/notify3.png">
</p>

* From the above image , we can see **slack_webhookurl,slack_username,slack_channel,slack** are madatory to set of a slack notification using **notify**

**Note** : Follow the below steps to create a Slack account and Generate WebhookURL.

**Step 1:** [Sign Up](https://slack.com/get-started#/create) for a slack account and verify the account.

**Step 2:** During the process , it will ask you to setup the **Team Name , Channel Name , Invites**.

<p align="center">
  <img src="/images/recon/notify4.png">
</p>

**Step 3:** Navigate to this URL <https://api.slack.com/apps> and click on create app.

<p align="center">
  <img src="/images/recon/notify5.png">
</p>

**Step 4:** Enter the **App Name** and **Slack Workspace** as shown below.

<p align="center">
  <img src="/images/recon/notify6.png">
</p>

**Step 5:** Choose **Incoming Webhooks** under the App Name and set the Activate Incoming WebhookURL to **ON**.

<p align="center">
  <img src="/images/recon/notify7.png">
</p>

**Step 6:** In the bottom of the page you will be able to see **Webhook URLs for Your Workspace**, click on **Add  New Webhook to Workspace**.

<p align="center">
  <img src="/images/recon/notify8.png">
</p>

**Step 7:** It will ask you for the permission to access slack workspace and the **channel name** and click on **allow**.

<p align="center">
  <img src="/images/recon/notify9.png">
</p>

**Step 8:** Now your slack webhookurl is generated as shown below.

<p align="center">
  <img src="/images/recon/notify10.png">
</p>

* We have got the Slack WebhookURL , Slack Username and Channel name.

<p align="center">
  <img src="/images/recon/notify11.png">
</p>

* Add these into the **notify.conf** , To edit the file use the following command.

`vi .config/notify/notify.conf`

* Uncomment the following fields `slack_webhook_url, slack_username, slack_channel, slack` and add the necessary details.

<p align="center">
  <img src="/images/recon/notify12.png">
</p>

<p align="center">
  <img src="/images/recon/notify13.png">
</p>

* Sample command to check the configuration works fine.

`curl -X POST -H 'Content-type: application/json' --data '{"text":"Hello, World!"}' https://hooks.slack.com/services/**********************************`

<p align="center">
  <img src="/images/recon/notify14.png">
</p>

* Scan for subdomains, check alive domains and send slack notifications using **notify** use the following command.

`subfinder -d hackerone.com | httpx | notify`

<p align="center">
  <img src="/images/recon/notify2.png">
</p>

<p align="center">
  <strong>Anew</strong>
</p>

* [Download](https://github.com/tomnomnom/anew) and install this Tool using the following command

`go get -u github.com/tomnomnom/anew`
 
* This tool is used to compare the output with old file and give us what is newly added in the file

For example `domain.txt` contains a set of sub-domains and now `subs.txt` same set of subdomains but included few additional extra new subdomains.

* Using the following command we can compare both the files and get what is newly added.

`cat subs.txt | anew domain.txt`

<p align="center">
  <img src="/images/recon/anew.png">
</p>

<p align="center">
  <strong>Security Through Intelligent Automation</strong>
</p>

* Now by using the above all tools we are going to create a **One Linear**. **subfinder** to find new subdomains , check wheather it is alive or not using **httpx** then scan it with **nuclei** and send us a slack notification of the output using **notify**.

**First Run** 

`subfinder -silent -dL domains.txt | anew subs.txt`

Note : `-dL` - File containing list of domains to enumerate.

**Second Run** 

`while true; do subfinder -dL domains.txt -all | anew subs.txt | httpx | nuclei -t nuclei-templates/ | notify ; sleep 3600; done`

Note : `while true; do` - Keeps the script alive.
       `sleep 3600; done` - Run after exactly every one hour.
        
* The **First Run** is madatory for this process so that it will do subdomain enumeration for the list of domains that you give and save it in a `.txt` file.

* The **Second Run** is used to find new subdomains , check alive status , scan it with nuclei templates and notify us the output.

Note : I highly suggest to run these one linear on VPS system like Digital Ocean or AWS instance.

<p align="center">
  <img src="/images/recon/automation.png">
</p>

<p align="center">
  <strong>Why Automation is needed in reconnaissance ?</strong>
</p>

* The chances of getting **duplicates** is **very less** and you will be focusing more on **NEW subdomains**

**For Example**

**Real Scenario**

* I got an alert at 3:54 PM Feb 05, 2021

<p align="center">
  <img src="/images/recon/slack1.png">
</p>

* I reported the vulnerability to the program at 3:59 PM Feb 05, 2021

<p align="center">
  <img src="/images/recon/slack2.png">
</p>

* Reported got Triaged and Valid !!!

Huge Shout out to **[Project Discovery Team](https://projectdiscovery.io/)** for creating Amazing Tools

<p align="center">
  <img src="https://media.giphy.com/media/YVGeZszGz4eC4/giphy.gif">
</p>

Kudos to [tomnomnom](https://github.com/tomnomnom) for anew tool and [pry0cc](https://twitter.com/pry0cc) for the Onelinear.

Thanks a lot for reading !!!.
