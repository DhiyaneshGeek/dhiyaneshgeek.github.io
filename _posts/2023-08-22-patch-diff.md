---
layout: post
title: Patch Diff
date: 2023-08-22
summary: Reviewing Code Changes
categories: Research
tags: [Reversing, N Day, Zero Day]
---

Hey, Everyone!  
     I'm back with blog post, and this time I want to share my experience on **Code Review**, it all started from doing **PentesterLab** [Code Review Badge](https://pentesterlab.com/exercises/codereview/course), It provides a solid foundation by initially focusing on understanding smaller code sections, then advancing to the comparison of code changes through patch differentials, and ultimately culminating in a thorough grasp of the entire source code.          
     In this blog post we will be focus on _Patch Diff Analysis_ part.

**What is Patch Diff ?**

<p align="center">
  <img src="/images/patch/patch-diff-logo.png"> 
</p>

**"patch"** or **"diff"** , is a utility used in software development and version control systems to compare two sets of files and identify the differences between them. The term "diff" is short for "difference.”

To generate a patch diff using the `diff` command in `Unix/Linux`, you typically use the following format:

```bash
diff -u <original_file> <modified_file> > <patch_file>
```
