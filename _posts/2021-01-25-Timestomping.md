---
layout: post
title: Was there timestomping on the analyzed system?
date: 2021-01-25 23:50
permalink: /IRC/timestomping
---

### What is timestomping?<br>
Timestomping is the ability from an attacker to modify original timestamps of folder/file in order to prevent the investigator from associating these timestamps with an attack period.

***
### Master File Table (MFT)
Filesystem usually has an index mechanism where you can see files/directories names in the system and their associated metadata. This includes timestamps.
In NTFS filesystem, each folder/file has one entry in MFT with two different timestamps called $STANDARD_INFORMATION and $FILE_NAME.

Mainly, standard information attributes are modifiable by users, whereas file name attributes tend to be modified by OS.

**MFT Location**<br>
Root directory of the drive for example C:\\$MFT

**MFT Parsing Tools**<br>
- MFTECMD<br>
- analyzeMFT.py<br>
- Mft2Csv<br>
- MFTExplorer<br>
- etc.<br> 

**Example**<br>
Let's check 1st easy challenge (Time Machine) in (incident-response-challenge.com) website and try to solve it.

Let's use MFTECmd to parse $MFT file, and then view the results using Timeline Explorer

Link to download these two tools: https://ericzimmerman.github.io/#!index.md


**MFTECmd used command**<br>
`MFTECMD.exe -f "$MFT" --csv . --bn MFTOUTPUT.csv`

. means we would like to save the outputted CSV file to the same directory.

---

### 1st Challenge

The challenge focuses on changes on user's desktop and the provided evidence is $MFT file; 
therefore, we have parsed the file using MFTECMD. Now, we'll view the parsed MFT using Timeline Explorer

**Question Screenshot**<br>

![first screenshot]({{site.baseurl}}/assets/images/210125-1.png)

Timeline explorer after opening parsed $MFT<br>

![second screenshot]({{site.baseurl}}/assets/images/210125-2.png)

In order to have a better view, we'll reset column widths (Tools > Rest Column Widths (Ctrl+R))
Now, we'll filter to show only the user's desktop folder as the parent path


![third screenshot]({{site.baseurl}}/assets/images/210125-3.png)


You may notice I've brough the attention into mainly two columns which are Created0x10 and Created0x30

Created0x10 is standard info creation time (easy be tampered with)<br>
Created 0x30 is file name creation time (difficult to be tampered with).<br>
If they differ, TIMESTOMPING might have happened.<br>
Timeline Explorer doesn't show timestamp in Created0x30 if it has the same date and time of Created0x10 to ease investigator work.
	
We can clearly see that file name creation time (Createdx30) is in  the same timeframe other files in the same folder have been created. 
	
### Answer to the challenge
FileName: Mod-File.txt<br>
Time Stamp: 19-01-2020 11:51:19
	

