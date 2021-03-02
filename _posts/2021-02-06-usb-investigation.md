---
layout: post
title: Was a USB connected to system?
date: 2021-02-06 13:45
permalink: /IRC/usb-investigation
categories: [IRC, Digital Forensics]
---

Sometimes, we would like to investigate if a USB connected to a system, and provide information related to the USB such as its
- Maker
- Serial Number/UID
- USB Connection Date/Time

---

### USB Connection Investigation
To investigate if a USB was connected or not, SYSTEM registry hive can be parsed looking for USBSTOR registry key.


**System Registry Files Location**
: %Windir%\System32\Config<br>
: One of which is SYSTEM hive


**USBSTOR Registry Key Location inside SYSTEM hive**
: SYSTEM\CurrentControlSet\Enum\USBSTOR


**Note**
I have parsed SYSTEM hive and I noticed ControlSet00* and not CurrentControlSet<br>
: ControlSet001 is the last control set the system booted with<br>
: ControlSet002 is the last known good control set<br>
<br>

**USB Investigation Tools**
: Registry explorer (SYSTEM Hive)<br>
: USB Detective<br>
: RECmd<br>
: RegRipper<br>
: etc.<br>
<br>

**Hunt for USB Connection**
If you would like to know whenever a new external device is connected or enabled, look for this event ID<br>
: - Security Logs, Event ID: 6416

**Example**
Let's check 2nd easy challenge (Hello DoK) in (incident-response-challenge.com) website and try to solve it.

Used tool here will be Registry Explorer

---

### 6th Challenge

This challenge focuses on proving whether a USB was connected to system, and if so, provide its serial/UID number.

**Question Screenshot**

![first screenshot]({{site.baseurl}}/assets/images/210206-1.png)

We'll parse SYSTEM hive using Registry Explorer and navigate to the following key USBSTOR
SYSTEM\ControlSet001\Enum\USBSTOR

![second screenshot]({{site.baseurl}}/assets/images/210206-2.png)

Maker: SanDisk

### Answer to the challenge
Serial/UID: 4C530000281008116284

***
### USB Connection Dates/Times
Using the same registry key USBSTOR, we are able to know more information with regards to USB connection/removal dates/times
- The first time this USB was connected
- The last time this USB was connected
- The last time this USB was removed

The followng screenshot shows these evidence

![third screenshot]({{site.baseurl}}/assets/images/210206-3.png)

**References**
1.https://docs.microsoft.com/en-us/windows/security/threat-protection/auditing/event-6416
2.https://www.13cubed.com/downloads/dfir_cheat_sheet.pdf