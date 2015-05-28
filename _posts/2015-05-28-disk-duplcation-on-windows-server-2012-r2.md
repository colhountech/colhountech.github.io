---
title: Data Deduplication on Windows Server 2012 R2 Rocks!
layout: post
published: true
---

I'm a bit of a stickler for backups, and just to be sure, I usually setup my traditional hard drives as Raid-1 mirrors. It's twice the cost but it just gives me that little bit more reassurance. Besides, who really wants to spend half a day rebuilding a server and mucking about with restoring backups?

My main dev machine, CHAOS was running out of space...again.  Recently I setup a secondary server purely for storage and moved a lot of devops stuff over there. For example, I have One Drive and One Drive for Business installed on my user account on that server, and since that's my Home Folder on the AD domain, it means that they are accessible as my Z: drive for all other machines on the network. Net result is that I only have to install One Drive and One Drive for Business on a single machine  and all my sync problems have gone away. It's really great! STORAGE is running a few other projects too.. It's acting as a Software SAN for the dev SQL Cluster, and I'm using it for my local code repository too - although, I have to say I am more likely to use GitHub or Visual Studio Online for that nowadays.

But still CHAOS was running low on space. I had about 100GB left on my 1.8GB RAID-1 D: drive. At the back of my mind, I had been thinking about Data Deduplication for a while now, but never gave it serious consideration. After all, I know my data and I'm pretty sure that apart from a couple of iTunes music archives that are mosty probably out of sync, I don't have much duplicate data. At least that's what I though...

I remember attending a seminar on the release of Windows Server 2012 R2 a few years back, when I first heard about Data Deduplicatio, but I never got a chance to try it out. Seemed like the timing was perfect.

I installed the Role under File and Storage Services > File and iSCSI Services > Data Deduplication. Done in a few seconds? Is that it?

![Installing the Data Deduplication feature] ( http:///images/Data%20Deduplication%20%20Feature.PNG )

It took a quick google on bind, and then all that is left is to navigate to the Server Manager > File and Storage Services > Volumes, Right-Click and select **Configure Data Deduplication**. I set the schedule to be quite agressive and to update twice per day, but I didn't even notice any performance impact to be honest. I took a before and after screen shot of the disk usage.

![Before] ( http:///images/Data%20Deduplication%20%20Before.PNG)

Next morning I was pleasantly surprised to see the 102GB free space had increased to over 550 GB, giving me a whopping DeDuplication rate of 23%.  Happy Days!

![After] ( http:///images/Data%20Deduplication%20After.PNG)

What I love about features such as this, is that they are so easy to setup and configure. 

Sl&aacute;n go Foil.














