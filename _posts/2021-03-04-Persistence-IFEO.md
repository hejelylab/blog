---
layout: post
title: How to find out if there is persistence using Image File Execution Options (IFEO)?
date: 2021-04-09 17:15
permalink: /IRC/Persistence-IFEO
categories: [IRC, Digital Forensics, Persistence]
---

### What is Image File Execution Options (IFEO)?
IFEO is a feature which lets developers attach a debugger to an application/process. This allows to run the debugger/application at the time of running the application we wish to debug.<br>

**How to set IFEO?**
- Using the registry<br>
- Using GFlags tool<br>


### IFEO Types with implementation

**First Implementation**
 Create a debugger to a process in this registry key<br>

> HKLM\Software\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\\\<ProcessName>

ProcessName is a registry key that has the name of the process we would like to attach a debugger to it.
The following example tells the debugger for notepad.exe will be calc.exe

**Example**
Initially, IFEO Key looks like this
Now, we'll add a key named notepad.exe
![first screenshot]({{site.baseurl}}/assets/images/210409-1.png)


As below, Debugger is now calc.exe, whenever notepad.exe is set to be executed, the debugger "calc" will be run
![second screenshot]({{site.baseurl}}/assets/images/210409-2.png)

**Second Implementation**
Launch a process/program when another application silently exits<br>
> Silent exit for an application means the application has been terminated in one of two ways
: Self termination by calling ExitProcess
: Another process terminates the monitored process by calling TerminateProcess

This can be set in the following registry key
> HKLM\Software\Microsoft\Windows NT\CurrentVersion\SilentProcessExit

**Example**
We'll run calc.exe once notepad.exe is silently exiting.<br>
First, we'll enable silent process exit monitoring by adding GlobalFlag name with hexadecimal value of 200 in notepad.exe key under IFEO key

![third screenshot]({{site.baseurl}}/assets/images/210409-3.png)

We'll create SilentProcessExit key under CurrentVersion, and under this key we'll add subkey named notepad.exe

By adding both 
MonitorProcess value to be calc.exe, and ReportingMode to 1,
now every silent exit of notepad.exe will trigger calc.exe to be run.

![fourth screenshot]({{site.baseurl}}/assets/images/210409-4.png)

An example of how this appears

![fifth screenshot]({{site.baseurl}}/assets/images/210409-5.gif)

Example
Let's check 7th medium challenge (Universal) in (incident-response-challenge.com) website and try to solve it.

Used tools here will be Registry Explorer to navigate through the registry keys

---

### 11th Challenge

This challenge tells an occasional popup for CMD happens which seems to be a persistence pattern.

![sixth screenshot]({{site.baseurl}}/assets/images/210409-6.png)

We'll check both registry keys that we have mentioned using Registry Explorer as the evidence mentions Global Flags in the provided evidence

System Regsitry hives are in C:\Windows\System32\config, we'll only parse Software registry hive

In Image File Execution Options, we notice the value name GloabalFlag exists

![seventh screenshot]({{site.baseurl}}/assets/images/210409-7.png)


And under SilentProcessExit key, and notepad.exe subkey, we notice the monitoring process is an executable under temp directory which will run whenever a silent exit occurs for notepad.exe


![eighth screenshot]({{site.baseurl}}/assets/images/210409-8.png)

### Answer to the challenge
ZmxhZy17Rm91bmRJdH0.exe


**References**
1. https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/gflags
2. https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/registry-entries-for-silent-process-exit
3. https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/enable-silent-process-exit-monitoring
4. https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/registry-entries-for-silent-process-exit
