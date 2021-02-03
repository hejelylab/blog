---
layout: post
title: Was a specific file deleted from filesystem?
date: 2021-02-03 19:15
permalink: /IRC/file-deletion-evidence
---

Sometimes, we would like to prove if a specific file existed at sometime in filesystem, and then got deleted. That is, the received image doesn't have the file we're looking for.

---

### Journaling
Filesystems have the concept of journaling which allows the OS to keep a track of changes that are made to the filesystem itself.

**NTFS FileSystem**<br>
NTFS filesystem has two journal types<br>
1. USN Journal<br>
	Purpose: keep track of changes to files/directories in the filesystem with the reason of each and every change<br>
	Location: C:\\$Extend\\$USNJOURNAL<br>
	$USNJOURNAL contains both $MAX and $J files<br>
	What we're mostly interested in is the $J file where a track of changes to files and folders are recorded
2. LogFile ($LOGFILE)<br>
	This file keeps a track of changes to $MFT file<br>
	

**Journals Parsing Tools**<br>
- ANJP<br>
- MFTECMD<br>
- etc.<br>

**Example**<br>
Let's check 2nd medium challenge (CopyPaSTe) in (incident-response-challenge.com) website and try to solve it.

Used tool here will be MFTECMD

**MFTECMD used command**<br>
`MFTECmd.exe -f "C:\Users\%username%\Downloads\Challenges\Medium - NTFS Journal Forensics - Copy PaSTe\Challenge\Artifacts-J\$J" --csv . --csvf J_CSV.csv`

-f points to $J file<br>
I've already downloaded the question evidence into my current Downloads folder.

--csv . means create csv file in the same current directory<br>
--csvf means name the outputted csv file this way

---

### 5th Challenge

This challenges focuses on proving whether a specific file related to (John's E-Mail data) has existed previously on Theon's system.

**Question Screenshot**

![first screenshot]({{site.baseurl}}/assets/images/210203-1.png)

Since the question's artifacts are only $MFT and Journaling files.
We'll parse $J using MFTECMD to find out if such a file existed on the system or not using the mentioned above command

As we can see in the outputted CSV file after some filtration to only include files that have the word "john", the following entries are seen. Evidently, PST file was created and then deleted from the file system


![second screenshot]({{site.baseurl}}/assets/images/210203-2.png)

### Answer to the challenge
File Name: JohnSnowPST.pst<br>