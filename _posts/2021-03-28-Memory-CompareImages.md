---
layout: post
title: How to identify malicious processes by comparing an infected memory image with a clean baseline one?
date: 2021-03-28 00:00
permalink: /IRC/Memory-CompareImages
categories: [IRC, Digital Forensics, Memory Investigation]
---

### 13th Challenge

This challenge tells there is a malicious process in a memory image and asked if we can identify this by comparing the memory image against another clean memory image.

![first screenshot]({{site.baseurl}}/assets/images/210328-1.png)

First thing we will do is confirming the memory image profile using imageinfo against one of the two images<br>

`vol.py -f DESKTOP-HUB666E-20191101-155228.dmp imageinfo`

![second screenshot]({{site.baseurl}}/assets/images/210328-2.png)

- Now, since the challenge asks about suspicious process, we'll use processbl plugin which compares the processes of an images to processes of a baseline image

- Two options to be used in comparison, display
    - Ony unknown by using -U option
    - Only known by using -K option<br>
`DESKTOP-HUB666E-20200209-162404.dmp --profile=Win10x64_15063 processbl -B ../BaseLine/DESKTOP-HUB666E-20191101-155228.dmp -U 2>/dev/null`

![third screenshot]({{site.baseurl}}/assets/images/210328-3.png)


The suspicious process appears to be whatsapp with PID:5392, this is due to spwaning CMD process (3352)

### Answer to the Challenge:
whatsapp.exe

---

### Side note
With same approach with comparing processes between images, we can compare drivers and services using plugins<br>
- servicebl
- driverbl
	





