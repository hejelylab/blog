---
layout: post
title: What folders were accessed by a specific user?
date: 2021-02-13 16:55
permalink: /IRC/folder-access
---

Sometimes we would like to know when investigating a user profile, if that user has accessed important folders, for example.

---

###  What are Shellbags?
Shellbags are user's registry keys which help in storing user's view preferences of folders in Windows OS.

**Example**<br>
You visit a specific folder, modify the way items are presented in that folder. Once visiting the same folder again, the previous view preference is rendered from Shellbag registry keys.
Therefore, Shellbags may provide us an evidence of user's access to folders.

Shellbags keys are in the following DAT files in each user's profile (these two DAT files are considered user's registry files/hives)<br>
: NTUSER.DAT
: UsrClass.dat<br><br>

NTUSER.DAT and UsrClass.dat Locations in each user's profile<br>
> NTUSER.DAT<br>
: C:\Users\\\<username>\NTUSER.DAT<br>

> UsrClass.dat<br>
: C:\Users\\\<username>\AppData\Local\Microsoft\Windows\UsrClass.dat<br>


**Once parsing the mentioned two DAT files, Shellbag keys are in the following locations**<br>
: NTUSER.DAT\Software\Microsoft\Windows\Shell\BagMRU
: NTUSER.DAT\Software\Microsoft\Windows\Shell\Bags
: UsrClass.dat\Local Settings\Software\Microsoft\Windows\Shell\BagMRU
: UsrClass.dat\Local Settings\Software\Microsoft\Windows\Shell\Bags

**Shellbag keys description**
> BagMRU<br>
: Stores actual directory structures of accessed folders<br>

> Bags<br>
: Stores actual folder customization data (window size, layout type, etc.)<br>

**Shellbags Parsing Tools**<br>
: Windows ShellBag Parser<br>
: Shellbags.py<br>
: ShellBags Explorer (View both NTUSER.DAT and UsrClass.dat)<br>
: SBECmd<br>
: Registry Explorer<br>
: RECmd<br>
: etc.<br>

**Example**<br>
Let's check 1st medium challenge (Can't touch this) in (incident-response-challenge.com) website and try to solve it.

Used tool here will be ShellBags Explorer

---

### 7th Challenge

This challenge asks if "Projects" folder was accessed in a specific time frame, and if so, confirm the folder recreation timestamp.

**Question Screenshot**

![first screenshot]({{site.baseurl}}/assets/images/210213-1.png)

Since the provided evidence contains user's profile files, we'll view both registry files NTUSER.DAT and UsrClass.dat using ShellBags Explorer searching for "Projects" folder.

![second screenshot]({{site.baseurl}}/assets/images/210213-2.png)

### Answer to the challenge
12:41:26