---
layout: post
title: How to find out if there is persistence using Registry run keys or Startup Folders?
date: 2021-03-02 18:15
permalink: /IRC/Persistence-Registry-Run-Keys
categories: [IRC, Digital Forensics, Persistence]
---

Out of many persistence techniques, one of the most common ones is the usage of Registry run Keys or Startup Folders. This will cause an added application to be executed whenever a user logs in.

---

### What are the startup folders?
They are folders that are checked whenever each user logs in<br>
> First path is under each user's profile<br>
C:\Users\\<username\>\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup

> Second path is system wide path for all users<br>
C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp

<br>

### Registry-Run-Keys
> Most used run keys for persistence are <br>
HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run
HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\RunOnce
HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run
HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\RunOnce

<br>

**Registry Parsing Tools**
: Registry explorer (SYSTEM Hive)<br>
: RECmd<br>
: RegRipper<br>
: etc.<br>

**Example**
Let's check 7th easy challenge (Sports) in (incident-response-challenge.com) website and try to solve it.

Used tools here will be Registry Explorer to navigate through the registry keys

---

### 10th Challenge

This challenge asks to look at the user's profile "Sansa", as there might be something when waking up!

![first screenshot]({{site.baseurl}}/assets/images/210302-1.png)

We'll parse NTUSER.DAT of the mentioned user, and navigate to Run (User run key) using the existing bookmarked keys.

![second screenshot]({{site.baseurl}}/assets/images/210302-2.png)

We can see an executable runs everytime this user logs in, which is under public user profile 

![third screenshot]({{site.baseurl}}/assets/images/210302-3.png)

### Answer to the challenge
Frag-AGREWEHDFG.exe

**References**
1. https://attack.mitre.org/techniques/T1547/001/