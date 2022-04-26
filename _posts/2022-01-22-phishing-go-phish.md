---
layout: post
title: Phishing
date: 2022-01-22
summary: Set-up and run a Phishing Campaign using GoPhish
categories: Red Teaming
tags: [Bugs, Write-Ups, Red Teaming]
---

Hi Everyone

I'm back with another blog, i will cover how to Set-up and run a Phishing Campaign using GoPhish in a Red Teaming Engagement.

<p align="center">
  <img src="/images/phishing/gophish.jpeg">
</p>

**What is Phishing ?**

Phishing is a type of **social engineering** attack often used to steal user data, including **login credentials** and **credit card numbers**. 

It occurs when an attacker, masquerading as a trusted entity, dupes a victim into opening an email, instant message, or text message.

**Requirements**

* Route53 (Domain Registration)
* AWS EC2 Instance
* Mailgun Account

**Route53 (Domain Registration)**

* Create an AWS Account and Login into [AWS Console](https://console.aws.amazon.com).

* Search for **Route53**.

<p align="center">
  <img src="/images/phishing/route53.png">
</p>

* Click on **Register Domain**.

<p align="center">
  <img src="/images/phishing/register.png">
</p>

* Enter the Domain name and click on check.

<p align="center">
  <img src="/images/phishing/unavailable1.png">
</p>

* It shows the domain status is Unavailable, so we are gonna buy the domain which is available.

<p align="center">
  <img src="/images/phishing/available1.png">
</p>

* After purchasing the domain, the request goes to **Domain Registration in Progress**.

<p align="center">
  <img src="/images/phishing/pending1.png">
</p>

* If the payment is already done, it will taken an hours to reflect the changes in the [AWS Console](https://console.aws.amazon.com/route53/home#DomainRequests:)

* Successful domain registration looks like as shown below.

<p align="center">
  <img src="/images/phishing/done1.png">
</p>

**AWS EC2 Instance**

* Login into [AWS Console](https://console.aws.amazon.com).

* Search for **EC2**.

<p align="center">
  <img src="/images/phishing/ec2.png">
</p>

* Click on **Launch Instance**.

<p align="center">
  <img src="/images/phishing/launch.png">
</p>

* Choose **Ubuntu Server** (Free tier eligible) and proceed further as shown below.

<p align="center">
  <img src="/images/phishing/ubuntu1.png">
</p>

<p align="center">
  <img src="/images/phishing/ubuntu2.png">
</p>

* Click on **Review and Launch**.

<p align="center">
  <img src="/images/phishing/review.png">
</p>

Note: It prompts the user to generate or use existing private key for accessing the Ubuntu System later via SSH.

* Choose Create a new key pair.

<p align="center">
  <img src="/images/phishing/ubuntu3.png">
</p>

* Download a key pair.

<p align="center">
  <img src="/images/phishing/ubuntu4.png">
</p>

* After downloading the Private Key file, Launch the Instance.

<p align="center">
  <img src="/images/phishing/ubuntu5.png">
</p>

<p align="center">
  <img src="/images/phishing/ubuntu6.png">
</p>

**Configuring Security Group**

* Click on the Security Group under the EC2 Dashboard

<p align="center">
  <img src="/images/phishing/securitygroup.png">
</p>

* Create a new Security Group.

<p align="center">
  <img src="/images/phishing/newgroup.png">
</p>

* Enter the Basic Details.

<p align="center">
  <img src="/images/phishing/basic.png">
</p>

* Add the following Inbound Rules for SSH, DNS, HTTP ,HTTPS, GoPhish as shown below.

<p align="center">
  <img src="/images/phishing/inboundrule.png">
</p>

* Security Group successfully created.

<p align="center">
  <img src="/images/phishing/successful.png">
</p>

* Now we need to assign this newly created Security Group to the Ubuntu EC2 Instance that we created.

* Navigate to **Instances** ---> **Right Click on Instance** ---> **Security** ---> **Change Security Groups** . 

<p align="center">
  <img src="/images/phishing/right.png">
</p>

* Remove the Existing Security Group.

<p align="center">
  <img src="/images/phishing/remove.png">
</p>

* Add the Newly Created Rule from the Drop Down.

<p align="center">
  <img src="/images/phishing/add.png">
</p>

<p align="center">
  <img src="/images/phishing/add2.png">
</p>

* Click on save to apply the changes.

<p align="center">
  <img src="/images/phishing/add3.png">
</p>

**Connecting to EC2 Instance via SSH**

* Move the Private Key (.pem) to .ssh .

```bash 
mv Downloads/ubuntu.pem .ssh/
```

* Change the Permission of the (.pem) .

```bash
chmod 600 .ssh/ubuntu.pem
```

<p align="center">
  <img src="/images/phishing/terminal.png">
</p>

* SSH into the ubuntu server using the following command.

```bash
ssh -i .ssh/ubuntu.pem ubuntu@3.134.76.191 #mention your system public Ip Address
```
<p align="center">
  <img src="/images/phishing/ssh.png">
</p>

**MailGun Domain Verification**

* Create a [MailGun](https://www.mailgun.com/) Account.

Note: Mailgun allows to Add Domains only if the credit card verification is successful.

* Navigate to **Sending** ---> **Domains** as shown below.

<p align="center">
  <img src="/images/phishing/mailgun.png">
</p>

* Add new domain.

<p align="center">
  <img src="/images/phishing/adddomain1.png">
</p>

* We have to add the DNS record for sending.

<p align="center">
  <img src="/images/phishing/adddns1.png">
</p>

* So go back to the AWS Route 53 and Choose Hosted Zone Details.

<p align="center">
  <img src="/images/phishing/hostedzone1.png">
</p>

* Click on create record and add those two TXT as shown below.

<p align="center">
  <img src="/images/phishing/createrecord1.png">
</p>

<p align="center">
  <img src="/images/phishing/txt11.png">
</p>

<p align="center">
  <img src="/images/phishing/txt22.png">
</p>

* Verify DNS Setting will to verify the setting is updated properly or not.

<p align="center">
  <img src="/images/phishing/verifydns.png">
</p>

<p align="center">
  <img src="/images/phishing/dnsupdated1.png">
</p>

* To verify the TXT record manually using the following command.

```
dig geekfreak.click TXT
```

<p align="center">
  <img src="/images/phishing/dig1.png">
</p>

**Installing GoPhish**

Note: Make sure to install golang

```bash
apt install golang
```

* Clone the [GoPhish](https://github.com/gophish/gophish.git) Github Repository using the following command

```bash
git clone https://github.com/gophish/gophish.git
```
* Edit the **config.json** change the 127.0.0.1:3333 ---> 0.0.0.0:3333

<p align="center">
  <img src="/images/phishing/editing.png">
</p>

* Build and Run gophish using the following command.

```bash
go build #building the binary
```

```bash
sudo ./gophish #run the binary
```

<p align="center">
  <img src="/images/phishing/password.png">
</p>

Note: username and password can be found from the logs after running the gophish binary.

* Navigate to https://ipaddress:3333 to access the GoPhish Dashboard (Make sure to add https://)

<p align="center">
  <img src="/images/phishing/dashboard.png">
</p>

* After the login, it ask us to reset the credentials.

**Creating SMTP Credentials**

* Post verification of DNS of the domain in Mailgun.

* Click on the domain setting and choose **SMTP**.

<p align="center">
  <img src="/images/phishing/smtp1.png">
</p>

* Click on **Reset Password** and copy the password from clipboard.

<p align="center">
  <img src="/images/phishing/resetpassword.png">
</p>

<p align="center">
  <img src="/images/phishing/clipboard.png">
</p>

* Go to GoPhish **Dashboard** ---> **Sending Profiles**.

<p align="center">
  <img src="/images/phishing/sendingprofile.png">
</p>

* Enter the SMTP username and Password and send a Test Email

<p align="center">
  <img src="/images/phishing/testemail.png">
</p>

<p align="center">
  <img src="/images/phishing/testdummy.png">
</p>

<p align="center">
  <img src="/images/phishing/sentsuccess.png">
</p>

* Received the Test Email.

<p align="center">
  <img src="/images/phishing/emailreceived.png">
</p>

* After the Email Sent Successful, **save** the Email Sending Profile.

<p align="center">
  <img src="/images/phishing/saveprofile.png">
</p>

<p align="center">
  <img src="/images/phishing/profileadded.png">
</p>

**Installing evilginx2**

* Install [evilginx2](https://github.com/kgretzky/evilginx2) using the following command.

```bash
sudo apt-get -y install git make
git clone https://github.com/kgretzky/evilginx2.git
cd evilginx2
make
```

* Start **evilginx2**.

```bash
sudo ./bin/evilginx -p ./phishlets/
```

<p align="center">
  <img src="/images/phishing/evilnginx.png">
</p>

**Configuring evilginx2**

* Configure the domain name and ipaddress.

```bash
config domain geekfreak.click #the domain you bought for phishing
```

```bash
config ip 3.134.76.191 #your public ipaddress
```

<p align="center">
  <img src="/images/phishing/ip.png">
</p>

* Navigate to **Route53** and Create few **A** records as shown below.

<p align="center">
  <img src="/images/phishing/arecord.png">
</p>

<p align="center">
  <img src="/images/phishing/createdrecord.png">
</p>

* Verify manually using the following command.

```bash
dig live.microsoft.geekfreak.click A
```

<p align="center">
  <img src="/images/phishing/verify.png">
</p>

* Add the Phishlets.

```bash
phishlets hostname o365 live.microsoft.geekfreak.click
```

* To enable the o365 Phishlets.

```bash
phishlets enable o365
```

<p align="center">
  <img src="/images/phishing/ohyeah.png">
</p>

* Create lures and generate urls using the following command.

```bash
lures create o365 #after this command you will get a lures ID
```

```bash
lures get-url 0
```
<p align="center">
  <img src="/images/phishing/lures.png">
</p>

<p align="center">
  <img src="/images/phishing/hacked.png">
</p>

* Once someone enters the valid credentials, you will get those credentials in the logs.

<p align="center">
  <img src="/images/phishing/cred1.png">
</p>

```bash
sessions #list the number of captured sessions
```

<p align="center">
  <img src="/images/phishing/cred2.png">
</p>

```bash
sessions 3
```
<p align="center">
  <img src="/images/phishing/cred3.png">
</p>

**Running a GoPhish Campaign**

* Login into GoPhish Dashboard.

* Navigate to **Users & Group** and Click on **New Group**.

<p align="center">
  <img src="/images/phishing/newgroup.png">
</p>

* Enter the details of the people and email address.

<p align="center">
  <img src="/images/phishing/bulk.png">
</p>

<p align="center">
  <img src="/images/phishing/redteam.png">
</p>

* Next step is to create the email template.

* Click on **Email Templates** ---> **New Template**.

<p align="center">
  <img src="/images/phishing/email.png">
</p>

* Give the Name for the template and make a custom page as shown below.

<p align="center">
  <img src="/images/phishing/custom.png">
</p>

<p align="center">
  <img src="/images/phishing/preview.png">
</p>

* Click on **Save** Template.

<p align="center">
  <img src="/images/phishing/saved.png">
</p>

* Navgiate to **Campaigns** ---> **New Campaign**.

<p align="center">
  <img src="/images/phishing/campaign.png">
</p>

<p align="center">
  <img src="/images/phishing/letsdothis.png">
</p>

* Campaign started successfully.

<p align="center">
  <img src="/images/phishing/started.png">
</p>

<p align="center">
  <img src="/images/phishing/dash.png">
</p>

* This is how the recipients receive the email.

<p align="center">
  <img src="/images/phishing/emailreceived.png">
</p>

* Once the target users enter the credentials, it can be observed on the **evilginx sessions** and can be used to replay the _cookie sessions via browser_.

<p align="center">
  <img src="/images/phishing/joke1.png">
</p>

**Reference:-**    
[How to run a phishing attack simulation with GoPhish](https://www.techrepublic.com/article/how-to-run-a-phishing-attack-simulation-with-gophish/)     
[How to Set Up a Phishing Campaign with Gophish](https://resources.hacware.com/gophish-demo/)    
[Practical Phishing with Gophish](https://medium.com/airwalk/practical-phishing-with-gophish-7dd384ad1840)   
[Penetration Testing: Gophish Tutorial (Phishing Framework)](https://www.youtube.com/watch?v=S6S5JF6Gou0)  

<p align="center">
  <img src="https://media.giphy.com/media/hQL0xnCrnT3jXn8RJc/giphy.gif">
</p>

This post is only for **Educational Purposes**.

So please **don't test phishing** on the target company that **you don't have permission**.

Thanks a lot for reading !!!.
