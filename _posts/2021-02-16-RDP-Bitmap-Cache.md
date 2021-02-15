---
layout: post
title: What information can you retrieve from lateral movement performed via RDP?
date: 2021-02-16 01:20
permalink: /IRC/RDP-Bitmap-Cache
---

In case of an investigation which consists of lateral movement using RDP, one of the most important evidence we would like to investigate is RDP bitmap Cache files.

---

###  What is RDP bitmap cache?
When a user connects to another system using RDP, small size (bitmap) images are stored in their RDP profile files, so that once the same image is to be used in the session it can be fetched/pulled quicker. And the overall RDP session experience is enhanced in case of a slow connection. This artifact can help us sometimes in identifying what was the user seeing in their RDP sessions. 

**RDP Bitmap Cache Location (Every user profile)**<br>
C:\Users\\\<username>\AppData\Local\Microsoft\Terminal Server Client\Cache

**RDP Bitmap Cache Parsing Tools**<br>
: bmc-tools.py<br>
: > This is the only tool I have used so far and it does the job perfectly<br>
Tool Link: [bmc-tools](https://github.com/ANSSI-FR/bmc-tools)

**Example**<br>
Let's check 8th medium challenge (Notes) in (incident-response-challenge.com) website and try to solve it.

**bmc-tools used command**<br>
Note: Python needs to be installed beforehand

`mkdir RDPBitMapOutput`<br>
`python bmc-tools.py -s "C:\Users\%username%\Downloads\Challenges\Medium - BMCache - Notes\Challenge\littlefinger\AppData\Local\Microsoft\Terminal Server Client\Cache" -d RDPBitMapOutput -b`

mkdir to create a folder that contains the output of bmc-tools script<br>
-s to point to RDP bitmap cache folder<br>
-b will provide one bitmap image which aggregates all bitmap images.<br>

---

### 8th Challenge

This challenge tells that an access to Littlefinger's session has been gained. 
Later, the attacker connected to DC using vary-adm's account.
The provided evidence is the user's profile as well as eventlogs.

**Question Screenshot**

![first screenshot]({{site.baseurl}}/assets/images/210216-1.png)


One of the first things to have in mind is what the attacker was seeing when they gained an access to Littlefinger's sesion; Therefore, we'll parse the RDP Bitmap Cache folder to know this using the aforementioned commmand.

By looking at the aggregated bitmap image (it contains the word *collage* in the name of the file), we notice a note that contains the mentioned user vary-adm account and his password

![second screenshot]({{site.baseurl}}/assets/images/210216-2.png)

### Answer to the challenge
Uncutedition1@#
