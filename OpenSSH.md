# Installing OpenSSH on Ubuntu-Server 22.04.3 LTS

Download and Install SSH on the server

	sudo apt install openssh-server
Check that the service is running

	sudo systemctl status ssh

If OpenSSH was installed and not enabled on boot

	sudo systemctl daemon-reload
	sudo systemctl enable ssh.service
	sudo systemctl start ssh.service
	sudo systemctl status ssh

# Configure VB Network

Return ip for the NAT network adapter

	ip a

Edit adapter settings in VB

Settings-> Network

Attached to: NAT

Advanced-> Port Forwarding

Add port forwarding rule:

Name: SSH Port Forwarding Rule

Protocol: TCP

Host IP: 127.0.0.1

Host Port: 2222

Guest IP: 10.0.2.15 -> IP that we noted from adapter on server

Guest Port: 22

# Start SSH Session

Connect from host terminal

	ssh -p 2222 USERNAME@127.0.0.1

# Connect using PuTTY

Host Name (or IP address): 127.0.0.1

Port: 2222

Connection type: SSH

Under saved sessions name the session and save it to store this configuration for future use. I like to use putty so that I can save and edit apperence settings for each of my connections very easily.
