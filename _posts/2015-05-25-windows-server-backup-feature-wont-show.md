---
layout: post
title: "Windows Server Backup Feature won't show"
published: true
---
Here is a strange one. About to backup one of the ADs in the lab, and restore to new hardware.

Installed Windows Server Backup Feature and another feature at the same time. I

No errors, no warmings, but Windows Server Backup doesn't show in the Server Manager Dashboard under tools. 

I know it's at least partially installed because I can run wbadmin from a cmd prompt.

Found this [comment online](http://community.spiceworks.com/topic/324007-server-2012-windows-server-backup-not-showing "spaceworks.com").

>	Sorry, I hit submit in a frenzy of typing and fat-fingering. I wasn't quite 
>	done...
>		
>	Don't ask me why, but it seems like the install of Windows Server Backup is 
>	in some odd way related to the WSUS role. I know it sounds crazy, but hear me 
>	out... I first began experiencing this issue on a server running WSUS. With 
>	both installed, I was able to use wbadmin as well as the GUI without issue. I 
>	then decided to transfer my WSUS server to a different machine, so I 
>	uninstalled the role, which took all the Windows Server Backup stuff with 
>	it... but left it as "installed" in Add/Remove features.
>
>	So, to test, I uninstalled Windows Server Backup, and then installed both 
>	Windows Server Backup and Windows Update Services together, including all 
>	dependencies, et voila, Windows Server Backup re-appeared and was available 
>	again to use.
>		
>	I'm not suggesting there's a link between WSUS and Windows Server Backup. 
>	Rather, I'm suggesting there may be a bug in the Add/Remove Roles/Features 
>	system of 2012 with regard to one of the two services. I hope this helps.
	
Nothing to lose, and I took a stab in the dark that there a missing depeency for a multi-feature install. So, I installed Network Load Balancer and voila, Windows Server Backup appeared. Removed Network Load Balancer and all back to normal. How strange.

Removed Network Load Balancer, and Windows Server Backup disappeared again!

Removed Windows Server Backup. Reboot.  Reinstall the Windows Server backup features...

Guess what happens?...



