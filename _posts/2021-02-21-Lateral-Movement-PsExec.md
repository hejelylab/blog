---
layout: post
title: How to prove there was a lateral movement using PsExec via EventLogs?
date: 2021-02-21 19:45
permalink: /IRC/Lateral-Movement-PsExec
categories: [IRC, Digital Forensics, Lateral Movement]
---

In a digital forensics investigation, one of the important points to look for is lateral movement between systems in the environment. This post shows how to prove if there was lateral movement through Sysinternal PsExec tool using only Windows EventLogs as an evidence artifacts.

---

###  What is PsExec?
It's a tool that lets you execute processes on other systems.


**An example of PsExec executing commands remotely**
Let's say we want to open a CMD window on a remote system<br>
`psexec \\RemoteIP -u DomainName\UserName -p UserPassword cmd`<br>

> Break it down<br>
we have psexec already installed on our system, and we run it from CMD connecting to a RemoteIP system with a valid username/password there, and eventually start a CMD instance on the other system

<br>

**How does PsExec execute commands remotely?**
Through interacting with Service Control Manager (SCM) via the network either through 
- Remote Procedure Call (RPC) or 
- Server Message Block (SMB).<br>
<br>

**PsExec Execution Steps (Example)**
1. Authentication completed on the target system<br>
2. An Administrative share is mapped<br>
3. PsExec service binary (PSEXESVC) is copied to the mapped share<br>
4. PsExec communicates with SCM via the network to start the service binary (PSEXESVC)<br>
5. Starts the intended process/application on the target system<br>


***

### PsExec Detection via EventLogs

**Source System**
- Security logs<br>
    - Event ID: 4688 (PsExec Process Creation)
    -  Event ID: 4689 (PsExec Process has been exited)
    -  Event ID: 4648
	    -  Account Name(Under Subject section): the already logged on user on the source system
	    -  Account Name (Under Account whose credentials were used section): the used account on the target system
	    -  Target Server Name: target system
	    -  Process Name: Used process (if no change to PsExec name, the executable info will have PsExec)
	    -  Network Address: target IP

**Target System**
- System logs
	- Event ID: 7045 (PSEXESVC was installed)
	- Event ID: 7036 (PSEXESVC service state has changed)
		- This event should appear twice once service has started (Executing state), and the second time when the service gets stopped (Stopped state)
- Security logs
	- Event ID: 4624
		- Successful logon
	- Event ID: 4672
		- Special privileges assigned to new logon
	- Event ID: 5140
		- A network share object was accessed
	- Event ID: 5145
		- A network share object was checked to see whether client can be granted desired access
	- Event ID: 4656
		- A handle to an object was requested
	- Event ID: 4663
		- An attempt was made to access an object

**Eventlogs Parsing/Viewing Tools**
: EvtxEcmd<br>
: Event Log Explorer<br>
: Timeline Explorer<br>
: etc.<br>


**Example**
Let's check 4th medium challenge (Kiwi) in (incident-response-challenge.com) website and try to solve it.

Used tools here will be EvtxEcmd to parse eventlogs, and Timeline Explorer to view and filter eventlogs.

---

### 9th Challenge

This challenge tells an appearance of kiwi logo (Mimikatz) appeared on DESKTOP-HUB666E (172.16.44.135), this is probably due to lateral movement from other systems.

Provided evidence
	- DESKTOP-HUB666E eventlogs
	- WIN-IL7M7CC6UVU (DC) eventlogs

**Challenge Questions**
- Provide another domain user account used by attacker aside from King-Slayer<br>
- Provide target system IP when this user was used<br>

![first screenshot]({{site.baseurl}}/assets/images/210221-1.png)

To solve this challenge, we'll parse only Security and System eventlogs from the two systems provided eventlogs to detect if there is lateral movement occurred between them using PsExec<br>
<br>
**Parsing Steps for each machine**<br>
1. Copy only Security and System eventlogs into a directory named Logs2, for example.
2. Run EvtxECmd on each directory as mentioned below<br>

**EvtxEcmd used command**

> DESKTOP-HUB666E (172.16.44.135)<br>
`EvtxECmd.exe -d "C:\Users\%username%\Downloads\Challenges\Medium - PassTheHash - Event Logs - Kiwi\Challenge\KingSlayerHost- EventLogs\Logs2" --csv . --csvf KingSlayerHost.csv`
<br>

> DC (WIN-IL7M7CC6UVU) (172.16.44.132)<br>
`EvtxECmd.exe -d "C:\Users\%username%\Downloads\Challenges\Medium - PassTheHash - Event Logs - Kiwi\Challenge\DC-EventLogs\Logs2" --csv . --csvf DC.csv`


Let's view the two file logs using Timeline Explorer

To determine source of lateral movement we will use the mentioned event IDs 4688,4689, and 4648 looking for PsExec in them

This event "4648" shows the execution of PsExec on Feb 9

![second screenshot]({{site.baseurl}}/assets/images/210221-2.png)

Event time: 2020-02-09 13:59:13<br>
To break the event into what we know<br>
> Subject Account Name: KingSlayer (Already logged on user)<br>
Target User name: Daenerys (User account which will be used on the target system)<br>
Target Server Name: WIN-IL7M7CC6UVU<br>
Process Name: C:\\temp\\Niceone\\PSTools\\PsExec.exe<br>
Target IP Address: 172.16.44.132<br>

<br>
### Answer to the challenge
IP address of target machine: 172.16.44.132<br>
Username: Daenerys<br>

***

### Evidence from destination system

Security logs, Event IDs: 4624 & 4672<br>
Used pivot point: Daenerys

![third screenshot]({{site.baseurl}}/assets/images/210221-3.png)

System logs, Event IDs: 7045 & 7036<br>
Used pivot point: PSEXESVC

![fourth screenshot]({{site.baseurl}}/assets/images/210221-4.png)

> Note<br>
In this example PSEXESVC service name wasn't changed from default.
This can be changed; however, using -r option once executing PsExec in the source system.


**References**
1. The challenge used in this post belongs to [incident-response-challenge.com](https://incident-response-challenge.com/)
2. https://www.jpcert.or.jp/english/pub/sr/20170612ac-ir_research_en.pdf
3. https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4648
4. https://docs.microsoft.com/en-us/sysinternals/downloads/psexec