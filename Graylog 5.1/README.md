# Overview
Complete Graylog 5.1 install on Ubuntu-Server 22.04.3

This is a collection documentation for my Graylog instance, I have tried to be as detailed as possible. As you will see, you can always run into some unknown obstacles so be ready to do your reasearch if that happens.

To start this will be a single-node configuration. I will add more details about the settings of the server and the cluster as I move forward in the project.

The end goal is to have an "enterprise-like" cluster running in my virtual environment.

Begin with 'Defining System' to get started

Please feel free to share your thoughts, I always welcome feedback!

# Define Tool Versions
>[!TIP]
>I found it very helpful to define each component of the install to verify compatibility. Plus, this made it much easier to find the corresponding documentation I needed.

VirtualBox - 7.0.12
  
Ubuntu Server - 22.04.3 LTS
 
Graylog - 5.1
 
ElasticSearch - 7.10
 
MongoDB - 6.0


Installing all software with respect to 'Hints' or referenced service documentation found in the Graylog Ubuntu installation documentation to ensure compatibility, documentation here: 

https://go2docs.graylog.org/5-1/downloading_and_installing_graylog/ubuntu_installation.html


# Server Installation:

Hostname: Graylog-Server

Domain Name: myguest.virtualbox.org

Base Memory: 2 GB

Processors: 1 CPU

Disk Size: 50 GB

Network:

Loopback  = 127.0.0.1

Adapter 1 = NAT / IPv4: 10.0.2.15

Adapter 2 = NAT Network / IPv4: 10.0.2.4

If you are using DHCP you can retrieve your Netowrk information from the server terminal using 'ip a'

# Order of install:

1. Install OpenSSH {optional}

2. Install MongoDB 6.0

3. Install ElasticSearch 7.10.2

4. Install Graylog 5.1
