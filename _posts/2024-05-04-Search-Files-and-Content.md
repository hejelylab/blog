---
layout: post
title: Incident Response & Threat Hunting Series, Search Files & Content
date: 2024-05-03 19:15
permalink: /IRandTHSeries/SearchFilesandContent
categories: [Incident Response & Threat Hunting Series]
---

**Search Files & Content: Introduction**
This lesson will be an introductory overview for a short sub-series focused on the topic of file search and their contents within the AD Environment. The search method will vary based on the available data, whether it’s the file name, hash, or distinctive content of these files

**Video Link**
[IR & TH Series - Velociraptor EDR Search Files & Content: Introduction [ARABIC]](https://www.youtube.com/watch?v=c8Jc904VQXI)

***

**Search Files & Content: Known File Name**
In this lesson, we will discuss the first point in the series of searching for files and their contents at the AD Environment level. \
Let’s assume, for example, that we have a specific file name and we want to know whether it exists in our Environment or not, whether we have prior knowledge of the folder that contains it or not.

<br>

> The most important point in this lesson is that the MFT (Master File Table) is an open book that we can search directly without the need to obtain it or even parse its contents. Taking this point into consideration, we can increase the effectiveness of dealing with this type of search and avoid the conventional search for files.
<br>

**Video Link**
[IR & TH Series - Velociraptor EDR Search Files & Content: Known File Name [ARABIC]](https://www.youtube.com/watch?v=lik8OD6gPiY)

***

**Search Files & Content: Hash**
In this lesson, we will delve into the second point of the file search and its contents within the AD Environment series.

In the previous lesson, we explored how to search for files when we have prior knowledge of the file name. However, let’s assume that the file’s name has been changed, and its location is unknown. But we have complete knowledge of the file’s content and, consequently, its unique hash. \
In this scenario, the previous search method won’t be effective, and our next option will be to use the hash in several efficient ways to access the desired files.
<br>

**Video Link**
[IR & TH Series - Velociraptor EDR Search Files & Content: Hash [ARABIC]](https://www.youtube.com/watch?v=I6gJIyBI9r0)

***

**Search Files & Content: YARA**
In this lesson, we will explore the third point in the series of searching for files and their contents within the AD Environment.

In the previous lessons, we learned how to search for files:
1. By knowing the file name, whether we have prior knowledge of the folder containing it or not.
2. By having complete knowledge of the file’s content and its hash.

<br>

**But what if we don’t have all this information?**
> Let’s assume, for example, that the desired content is inside a compressed file, and we don’t know the file name or its location. In such cases, our expected option would be to search for distinctive content within files using YARA rules. 

<br>

**Video Link**
[IR & TH Series - Velociraptor EDR Search Files & Content: YARA [ARABIC]](https://www.youtube.com/watch?v=SHIfnCqIg0I)



