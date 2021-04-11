---
layout: post
title: Was a specific file accessed by this user?
date: 2021-01-28 21:00
permalink: /IRC/file-access-evidence
categories: [IRC, Digital Forensics]
---

Sometimes, we're encountered with this question of whether for example a compromised account has accessed an important local/remote file.

There are two important artifacts on Windows system that may reveal this information to us which are JumpLists and LNK files.

---

### What are JumpLists?
Jumplists are means to ease user's access to the frequently/previously accessed items in system with regards to the installed application.

This will provide us an opportunity to see if an an application file was opened under a user profile or not.

**JumpLists Locations**
They are in each user's pofile in these two directories<br>
C:\Users\\\<username\>\AppData\Roaming\Microsoft\Windows\Recent\AutomaticDestinations<br>
C:\Users\\\<username\>\AppData\Roaming\Microsoft\Windows\Recent\CustomDestinations<br>

**JumpLists Parsing Tools**
- Jumplist Explorer (GUI based Jump List viewer)<br>
- JLECmd (Jump List parser)<br>
- etc.<br> 

**Example**
Let's check 3rd easy challenge (Bling-Bling) in (incident-response-challenge.com) website and try to solve it.

Used tool here will be Jumplist Explorer.

---

### 2nd Challenge

This challenge focuses on proving whether one of the suspected users has accessed an xlsx file and provides as an answer both their first name as well as the accessed file name.

**Question Screenshot**

![first screenshot]({{site.baseurl}}/assets/images/210128-1.png)

Since the question artifacts are only the Jumplist directories (AutomaticDestinations and CustomDestinations) for two users, we'll import the content of these four directories into JumpList Explorer at once, and start navigating them.

![second screenshot]({{site.baseurl}}/assets/images/210128-2.png)

We can observe the finance related file appears under JSnow profile JumpList

### Answer to the challenge
FileName: Finance-Summary.rar<br>
First Name: John<br>


***

As mentioned above, the other important artifact to prove if a user has accessed/opened file is LNK files.

### What are LNK files?
LNK files are Microsoft Windows Shortcut Files.
These LNK files usually get created once opening a local/remote file.

This can also be helpful sometimes even if the original (target) file has been removed.

**LNK files Locations**
They can be anywhere in filesystem; however, the following two directories are interesting ones to parse when trying to seach for an evidence related to whether a file has been accessed/opened or not.

C:\Users\\\<username\>\AppData\Roaming\Microsoft\Windows\Recent<br>
C:\Users\\\<username\>\AppData\Roaming\Microsoft\Office\Recent

**LNK Files Parsing Tools**
- LECMD<br>
- exiftool<br>
- etc.

**Example**
Let's check 8th easy challenge (LNK Files) in (incident-response-challenge.com) website and try to solve it.

Used tool here will be LECMD

**LECMD used command**
`LECmd.exe -d "C:\Users\%username%\Downloads\Challenges\Easy - LNK - Rumors\Challenge\littlefinger\AppData\Roaming\Microsoft\Windows\Recent" --csv . --csvf littlefingerLNK.csv`

`-d` directs lecmd to parse recent folder of littlefinger user.<br>
I've already downloaded the question evidence into my current Downloads folder.

`--csv .` means create csv file in the same current directory<br>
`--csvf` means name the outputted csv file this way

---

### 3rd Challenge

This challenge focuses on proving whether a specific user (Littlefinger) has accessed the salaries file or not.

**Question Screenshot**

![third screenshot]({{site.baseurl}}/assets/images/210128-3.png)

Provided evidence is the suspected user's profile; therefore, we'll check LNK files and parse them using LECMD.

As we can see in the outputted CSV file after some filtration to only include files that have network directory since we're only interested in network share access


![fourth screenshot]({{site.baseurl}}/assets/images/210128-4.png)

### Answer to the challenge
F1a9-AFNIEJFJSSE

**References**
1. The challenge used in this post belongs to [incident-response-challenge.com](https://incident-response-challenge.com/)