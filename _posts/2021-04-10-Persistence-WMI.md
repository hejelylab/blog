---
layout: post
title: How to find out if there is persistence using WMI?
date: 2021-04-10 17:16
permalink: /IRC/Persistence-WMI
categories: [IRC, Digital Forensics, Persistence]
---

If you would like to know about WMI, this post explains [WMI in Details](https://hejely.github.io/blog/DigitalForensics/WMI)

### 14th Challenge

This challenge states there is a persistence mechanism that's related to powershell and CMD.

![first screenshot]({{site.baseurl}}/assets/images/210410-1.png)

We'll parse OBJECTS.DAT WMI repository file using PyWMIPersistenceFinder.py
You can find the tool here: [PyWMIPersistenceFinder](https://github.com/davidpany/WMI_Forensics)

![second screenshot]({{site.baseurl}}/assets/images/210410-2.png)

### Answer to the challenge
C:\temp\addadmin.ps1
