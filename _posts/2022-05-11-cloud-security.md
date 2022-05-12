---
layout: post
title: Cloud Security
date: 2022-05-11
summary: AWS Misconfigurations
categories: Cloud Security
tags: [AWS, Write-Ups, Cloud Security]
---

Hi Everyone

I'm back with another blog on Deep Dive into AWS Cloud Security from scratch.


<p align="center">
  <img src="/images/cloud/aws-logo.png">
</p>

**What is Cloud Security ?**          
* Cloud security is a set of policies, strategies, controls, procedures, and practices designed to **safeguard** the data, resources, and applications hosted on the **cloud**.

**Why to Learn Cloud Security ?**
* Cloud security is critical since most **organizations** are already using cloud computing in one form or another. 
* Worldwide **end-user** spending on public cloud services is forecast to grow **20.4% in 2022** to total **$494.7 billion**, up from **$410.9 billion** in **2021**, according to the latest forecast from [Gartner, Inc](https://www.gartner.com/en/newsroom/press-releases/2022-04-19-gartner-forecasts-worldwide-public-cloud-end-user-spending-to-reach-nearly-500-billion-in-2022#:~:text=IaaS%2C%20DaaS%20and%20PaaS%20to,latest%20forecast%20from%20Gartner%2C%20Inc.)

<p align="center"><strong>Top Cloud Providers</strong></p>
<p align="center">
  <img src="/images/cloud/cloud-provider.png">
</p>

From the above statistics, it shows **Amazon AWS** dominates the cloud industry, so i decided to start with _AWS Cloud Security_, but :(

<p align="center">
  <img src="https://media.giphy.com/media/8VIRj0jsCPIIBpKSDK/giphy.gif">
</p>

So i started to look where i can **create and deploy** a **vulnerable** cloud enviroment for learning,i end up by finding [CloudGoat](https://github.com/RhinoSecurityLabs/cloudgoat).

<p align="center">
  <img src="/images/cloud/vulnerable-labs.png">
</p>

<p align="center"><strong>CloudGoat(☁️🐐)</strong></p>      

<p align="center">CloudGoat is Rhino Security Labs "Vulnerable by Design" AWS deployment tool.</p>

<p align="center">
  <img src="https://rhinosecuritylabs.com/wp-content/uploads/2018/07/cloudgoat-e1533043938802-1140x400.jpg" width=350/>
</p>

**Set-Up Requirements**
1. Linux or Mac OS
2. Python 3.6+
```bash
apt install python3
```
3. [Terraform](https://www.terraform.io/downloads)
```bash
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
sudo apt-get update && sudo apt-get install terraform
```
4. [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```
5. [jq](https://stedolan.github.io/jq/)
```bash
apt install jq
```
**Creating an AWS Administrator Account**
* [Sign Up](https://portal.aws.amazon.com/billing/signup#/start/email) for an Amazon AWS Account.
* After login, search for Identity & Access Management (IAM).
* Choose Users ---> Add users

<p align="center">
  <img src="/images/cloud/user.png">
</p>

* Set **username** and choose AWS Credentials Type as **Programmatic Access**.

<p align="center">
  <img src="/images/cloud/nameusers.png">
</p>

* Choose **Attach existing policies directly** ---> **AdministratorAccess**.

<p align="center">
  <img src="/images/cloud/permission.png">
</p>

* Give any tag name as shown below.

<p align="center">
  <img src="/images/cloud/tag.png">
</p>

* **Review** all the setting once and click on **Create User**.

<p align="center">
  <img src="/images/cloud/review.png">
</p>

<p align="center">
  <img src="/images/cloud/download.png">
</p>

**Configuring AWS Profile**

* After creating the AWS Administrator Account use the following command to configure the AWS profile.

```bash
aws configure --profile cloudgoat #give any profile name
```

<p align="center">
  <img src="/images/cloud/awsprofile.png">
</p>

* Check if the user is configured with the AWS access key and Secret Key to the corresponding profile.

```bash
aws iam get-user --profile cloudgoat
```

<p align="center">
  <img src="/images/cloud/get-user.png">
</p>

**Installing CloudGoat**

To install CloudGoat, make sure your **system meets** the **requirements** above.

```bash
git clone https://github.com/RhinoSecurityLabs/cloudgoat.git
cd cloudgoat
pip3 install -r ./requirements.txt
```
