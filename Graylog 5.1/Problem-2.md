# Overview
After installing mongoDB 6.0 I was getting a failure status trying to run the service. 

Error message:

	mongod.service - MongoDB Database Server
	     Loaded: loaded (/lib/systemd/system/mongod.service; disabled; vendor preset: enabled)
	     Active: failed (Result: core-dump) since Sun 2023-10-22 23:25:56 CEST; 2s ago
	       Docs: https://docs.mongodb.org/manual
	    Process: 86514 ExecStart=/usr/bin/mongod --config /etc/mongod.conf (code=dumped, signal=ILL)
	   Main PID: 86514 (code=dumped, signal=ILL)
	
	Aug 22 23:25:55 Ubuntu20-64Bit systemd[1]: Started MongoDB Database Server.
	Aug 22 23:25:56 Ubuntu20-64Bit systemd[1]: mongod.service: Main process exited, code=dumped, status=4/ILL
	Aug 22 23:25:56 Ubuntu20-64Bit systemd[1]: mongod.service: Failed with result 'core-dump'.

MongoDB was crashing before creating any logs so I was at a stand still, since I was unable to continue with the install.

This wa a great leaning moment as I spent about 4 hours reading through the forums from my many google searches, until I found my way back on to the MongoDB site forum where I eventually found my answer.

>[!TIP]
>The best place to look for answers to problems with a specific tool, is usually the forum hosted by the developer!

# Solutions

There was an issue with resource allocation, VirtualBox was not recieving AVX instuctions from the processor on the host machine. So, to VB it looked like I had incompatible hardware for this version of MongoDB.
In reality, the issue was that there was already virtualization services running on my host machine that were preventing VB from getting to the resource instructions it needed. The conflict Microsoft Hyper-V. I tried to
diasable Hyper-V through Powershell (Solution 1), but quickly found that the service would get turned back on on boot becuase of a new windows security feature. Diabling that feature (Solution 2) fixed my problem and 
I was finally able to get MongoDB to run. I had to diable features in my security settings to get this to work so please be careful and only do so at your own risk.

Solution 1:

Open PowerShell as an Admiistrator
Run the following command:

	Disable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-Hypervisor

Solution 2:

Open Windows Security

Device Security -> Core Isolation -> Core Isolation details

Toggle 'Memory integrity' to off

You can double check in VirtualBox if you are to access AVX instuction for virtualization by confirming that there is a symble of a chip with a blue 'V', if Hyper V is still enabled/running this symbol will be a 
turtle with a 'V' on its shell.
