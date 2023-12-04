# Overview

The network adapter was not receiving DHCP from the NAT Network. I am using a Nat Network to connect to the internet and to be able to connect 
to future VMs when adding to a cluster. I noticed that when running `ip a` to view adapter settings that the adapter was showing but had no ip 
assigned to it. I noticed in the details that the adapter showed its `state` as `down` and proceeded to search online for cause/solutions for 
why this was down on boot. After looking through several threads on askubuntu it turns out the adapter was missing from the netplan config.

# Solution

Edit the file at path: ```/etc/netplan/00-installer-config.yaml``` to add the network adapter that was not receiving an ip.

Open 'netplan' config

	sudo nano /etc/netplan/00-installer-config.yaml

Original Config

	network:
		ethernets:
    		eno2:
      			dhcp4: true
	  	version: 2

Updated Config

	network:
		ethernets:
			eno2:
				dhcp4: true
			enp0s8:
				dhcp4: true
		version: 2

Update `netplan` so the adapter shows up on boot, after edit to config

	sudo netplan generate
	sudo netplan apply

The adapter now receives ip from DHCP on NAT Network and internet is reachable, this can be verified by `ip a`      
