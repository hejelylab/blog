---
layout: post
title: Was a malicious application executed on system?
date: 2021-01-31 23:00
permalink: /IRC/application-execution-evidence-prefetch
---

Sometimes, we would like to answer once a malicious application found on system, the following questions

- Was it executed?
- How many times it was executed?
- What date/time the application was executed?


One of several found artifacts on Windows system that provide evidence whether an application was executed or not is Prefetch.

---

### What is Prefetch?
Prefetch is a mean for the OS to speed up booting and application loading times. 
This artifact can help us in identifying if an application was executed, time of execution, and number of times it was executed. 

All of this depends if this feature is enabled on system or not. Due to the low number of times Windows server boot up in comparison to workstations, this feature is disabled by default.

**Prefetch Directory Location**<br>
C:\Windows\Prefetch

**Prefetch Example**<br>
CMD.EXE-12345678.pf

Details

- Application Name<br>
- Dash -<br>
- Path Hash (8 Characters)<br>
- .pf<br>

> **Note**<br>
> Let's say you've found two entries relate to CMD, what does this mean?<br>
> This means that CMD.EXE is located in two different locations and both of CMD instances were run at least 1 time.

> **Note about Path Hash**<br>
> In case of the executed application is a hosting application such as rundll32.exe, svchost.exe, etc. the calculated hash isn't generated from only the path hash, but also other factors contribute into this such as the used command line when running the application, in addition to the /Prefetch command line argument if it was used.

**Prefetch Parsing Tools**<br>
- PECMD<br>
- WinPrefetchView<br>
- w10pf_parse<br>
- etc.<br> 

**Example**<br>
Let's check 5th easy challenge (B4 Catch) in (incident-response-challenge.com) website and try to solve it.

Used tool here will be PECMD

**PECMD used command**<br>
`PECmd.exe -f "C:\Users\%username%\Downloads\Challenges\Easy - Prefetches - B4-Catch\Challenge\Prefetch\SCVHOST.EXE-E4213C89.pf"`

---

### 4th Challenge

This challenges asks two questions with regards to found malicious application on system

- How many times this application was executed?
- Last execution time of the application


**Question Screenshot**

![first screenshot]({{site.baseurl}}/assets/images/210131-1.png)

Since the question is about only the malicious application "scvhost", we'll parse only its prefetch (pf) file.<br>
The following screenshots shows the number of times it was executed, and the times of execution, in addition to the last run.

![second screenshot]({{site.baseurl}}/assets/images/210131-2.png)

### Answer to the challenge
Number of execution: 4<br>
Time Stamp: 07-02-2020 21:26<br>

**References**<br>
1. https://www.hexacorn.com/blog/2012/06/13/prefetch-hash-calculator-a-hash-lookup-table-xpvistaw7w2k3w2k8/
2. https://hiddenillusion.github.io/2016/05/10/go-prefetch-yourself/