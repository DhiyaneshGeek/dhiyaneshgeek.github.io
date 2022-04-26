---
layout: post
title: Hopper Disassembler
date: 2021-12-25
summary: Bypassing Jail Break Detection
categories: Mobile Security
tags: [Bugs, Write-Ups, Mobile Security]
---

Hi Everyone

I'm back with another blog, i will cover the basic Jail Break Detection Bypass using Hopper Disassembler.

<p align="center">
  <img src="/images/hopper/hopper.jpeg">
</p>

**What is Hopper Disassembler?**

Hopper Disassembler, the reverse engineering tool lets you disassemble, decompile and debug iOS applications.

**Different ways to Bypass Jail Break Detection:**

* Objection
* Frida
* Hooking
* Tweaks(A-Bypass, Liberty etc)
* Reverse Engineering(Hopper Disassembler, etc)

Note : Make sure your device is Jail Broken and Cydia is Installed.

**Bypassing Jail Break Detection using Hopper Disassembler:**

**Step 1:** Download the [SecureStoreV2](https://mega.nz/folder/dJ1FUaYZ#Qx5tDEUEAaikBom9BviiwA) .ipa file and install the application.

<p align="center">
  <img src="/images/hopper/app.png">
</p>
  
Note: Since the application has jailbreak detection mechanism in it, it crashes and closes whenever the user tries to open the application, hence disabling the user to use this application on a jailbroken device.

**Step 2:** Unzip the **SecureStoreV2.ipa** using the following command.

  ```bash
  unzip SecureStorev2-resigned.ipa
  ```
<p align="center">
  <img src="/images/hopper/extract.png">
</p>
  
**Step 3:** Navigate into the **Payload -> SecureStorev2.app** Directory.

<p align="center">
  <img src="/images/hopper/new.png">
</p>

**Step 4:** Drag and Drop the **SecureStorev2** Binary in the Hopper Disassembler.

<p align="center">
  <img src="/images/hopper/drag.png">
</p>

<p align="center">
  <img src="/images/hopper/assembler.png">
</p>

**Step 5:** Use **str** search functionality to check for jailbreak keyword.

<p align="center">
  <img src="/images/hopper/keyword.png">
</p>

**Step 6:** Double clicking on the search result,it will take you directly to the **Assesmbly Instruction Set** as shown below.

<p align="center">
  <img src="/images/hopper/set.png">
</p>

**Step 7:** Right click and choose **isDeviceJailBroken -> References to "alsdevicejailbr"**.

<p align="center">
  <img src="/images/hopper/reference.png">
</p>

**Step 8:** Choose the first option from the dropdown list that appears.

<p align="center">
  <img src="/images/hopper/dropdown.png">
</p>

<p align="center">
  <img src="/images/hopper/another.png">
</p>

**Step 9:** Use the **Graph View** to see how the instructions sets works in details.

<p align="center">
  <img src="/images/hopper/graph.png">
</p>

<p align="center">
  <img src="/images/hopper/broken.png">
</p>

**Step 10:** From the red line, it can be observed that it executes the **suspend** and **exit** instruction set which causes the crash and closure of the application.

<p align="center">
  <img src="/images/hopper/block.png">
</p>

**Step 11:** The last Instruction **tbz** (Test bit and branch), it specifically test for a particular bit and takes a jump.

<p align="center">
  <img src="/images/hopper/tbz.png">
</p>

**Step 12:** We need to change the **tbz -> tbnz** which is the exact opposite of tbz instruction, so that the particular check will be skipped.

**Step 13:** Use hex option to view the arm instructions in Hex Format as shown below.

<p align="center">
  <img src="/images/hopper/hex.png">
</p>

<p align="center">
  <img src="/images/hopper/change.png">
</p>

Note: Hopper Disassembler Free Demo version does not allow to save file and create a new executable.

**Step 14:** Use vi & xxd (hexeditor) to edit the binary file.

```bash
vi SecureStorev2
```

<p align="center">
  <img src="/images/hopper/vi.png">
</p>

```bash
:%!xxd #covert the binary into hex
```

<p align="center">
  <img src="/images/hopper/xxd.png">
</p>

<p align="center">
  <img src="/images/hopper/editor.png">
</p>

```bash
/00c4f #search for 40 03 00 36 hex value
```

<p align="center">
  <img src="/images/hopper/search.png">
</p>

**Step 15:** Find and replace the **40 03 00 36** --> **40 03 00 37** which will result in tbnz instruction set.

<p align="center">
  <img src="/images/hopper/inside.png">
</p>

<p align="center"><strong>Before Changes</strong></p>

<p align="center">
  <img src="/images/hopper/outside.png">
</p>

<p align="center"><strong>After Changes</strong></p>

**Step 16:** Save the Binary using the following command.

```bash
:%!xxd -r
```

<p align="center">
  <img src="/images/hopper/reverse.png">
</p>


```
:wq! #save the binary file
```

<p align="center">
  <img src="/images/hopper/save.png">
</p>

Note : Make sure you remove the Old Binary and replace with the Edited One in the Payload Folder.

<p align="center">
  <img src="/images/hopper/old.png">
</p>

**Step 17:** Zip the Payload folder as shown below.

<p align="center">
  <img src="/images/hopper/payload.png">
</p>

**Step 18:** Rename the **.zip** file to **.ipa**

<p align="center">
  <img src="/images/hopper/rename.png">
</p>

<p align="center">
  <img src="/images/hopper/ipa.png">
</p>

**Step 19:** Install the Patched(Edited) app in the Jail Broken Iphone.

<p align="center">
  <img src="/images/hopper/securev2.png">
</p>

**Step 20:** From the below image, it can be observed that the application doesn't crash and Bypasses the Jail Break Detection Successfully.

<p align="center">
  <img src="/images/hopper/working.png">
</p>

**Reference:-**    
[How to bypass Jailbreak detection using Hopper Disassembler in iOS apps](https://www.youtube.com/watch?v=fW8ZleDki4U)     
[Jailbreak Detection and Bypass Techniques [iOS 12.2]](https://ub3rsick.github.io/2019/10/24/jb-detec-bypass/)                
[How to Hack/Patch Any iOS AppStore App | Reverse Engineering & ARM64 Assembly Tutorial](https://www.youtube.com/watch?v=bm1U-c-YVTw)            
[iOS Tampering and Reverse Engineering](https://github.com/OWASP/owasp-mstg/blob/master/Document/0x06c-Reverse-Engineering-and-Tampering.md)

<p align="center">
  <img src="https://media.giphy.com/media/8DihcJnSLzLHNfgf14/giphy.gif">
</p>

Thanks a lot for reading !!!.
