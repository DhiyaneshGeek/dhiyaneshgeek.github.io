---
layout: post
title: Patch Diff
date: 2023-08-22
summary: Reviewing Code Changes
categories: Research
tags: [Reversing, N Day, Zero Day]
---

Hey, Everyone! 

I'm back with blog post, this time will be focus on _Patch Diff Analysis_ part.

**What is Patch Diff ?**

<p align="center">
  <img src="/images/patch/patch-diff-logo.png"> 
</p>

**"patch"** or **"diff"** , is a utility used in software development and version control systems to compare two sets of files and identify the differences between them. The term "diff" is short for "difference.”

**Syntax for Diff**

```bash
diff <option> <original_file> <modified_file> > <patch_file>
```

<p align="center"><strong>Most commonly used diff options</strong></p>

| Option              | Description                                               | Example                                   |
|---------------------|-----------------------------------------------------------|-------------------------------------------|
| -u or --unified     | Unified context diff output                               | `diff -u file1.txt file2.txt`             |
| -r or --recursive   | Recursive comparison for directories                      | `diff -r dir1 dir2`                       |
| -w or --ignore-all-space | Ignore all white space when comparing lines          | `diff -w file1.txt file2.txt`             |
| --side-by-side      | Side-by-side comparison                                   | `diff --side-by-side file1.txt file2.txt` |
| --color             | Enable colored output to highlight differences            | `diff --color file1.txt file2.txt`        |

More options can be found here [Options to diff](https://www.gnu.org/software/diffutils/manual/html_node/diff-Options.html)

To generate a diff use the **diff** command in `Unix/Linux`, you typically use the following format:

```bash
diff -u <original_file> <modified_file> > <patch_file>
```
Here's what each part of the command means:

- `diff` : This is the command itself, which is used to compare files and generate the patch diff.
- `u` : This option tells **diff** to use the unified diff format, which is the most commonly used and human-readable format for patches.
- `<original_file>` : This is the path to the original file, the one you want to compare against.
- `<modified_file>` : This is the path to the modified file, the one you have made changes to.
- `>` : This is the output redirection symbol that saves the generated patch diff to a file.
- `<patch_file>` : This is the name of the file where the patch diff will be saved.

Let's illustrate this with an example. Suppose we have two text files, **file1.txt** and **file2.txt**, and we want to generate a patch diff (see what codes are changed) between them:

**file1.txt**

```bash
This is the original text.
```

**file2.txt**

```bash
This is the modified text.
```

We can use the **diff** command to generate the patch diff:

```bash
diff -u file1.txt file2.txt > my_patch.diff
```

This command will compare **file1.txt** and **file2.txt**, and the output will be redirected to a file named **my_patch.diff**. 

The content of **my_patch.diff** will look like this:

```bash
--- file1.txt	2023-07-23 10:00:00.000000000 -0400
+++ file2.txt	2023-07-23 10:00:00.000000000 -0400
@@ -1 +1 @@
-This is the original text.
+This is the modified text.
```

- **---** and **+++** indicate the file names and timestamps.
- **@@** provides information about the line numbers and context of the changes.
- **-** and **+** represent the lines removed and added, respectively.

In this example, the diff shows that the original line was removed, and the modified line was added.

**Reference:**   
* [An introduction to diffs and patches](https://opensource.com/article/18/8/diffs-patches)            
* [Patch Diffing](https://cve-north-stars.github.io/docs/Patch-Diffing)                
* [How to use diff and patch](https://www.pair.com/support/kb/paircloud-diff-and-patch/)           
* [Diff and Patch : A Guide](https://boseji.com/posts/diff-and-patch-a-guide/)    

<p align="center">
  <img src="https://media.giphy.com/media/BgKEiHf1xNV0h6IcSX/giphy.gif">
</p>

Thanks a lot for reading !!!.
