# Graylog 5.1

Install the Graylog repository

	wget https://packages.graylog2.org/repo/packages/graylog-5.1-repository_latest.deb
	sudo dpkg -i graylog-5.1-repository_latest.deb

# Install Graylog
Download to install Graylog

	sudo apt-get update && sudo apt-get install graylog-server 

# Edit Configuration file

Create root_password_sha2, you will enter your password for graylog after running this command to create the sha256 hash. This is also the password you will use to login to the web GUI

	echo -n "Enter Password: " && head -1 </dev/stdin | tr -d '\n' | sha256sum | cut -d" " -f1

Create your password_secret to 'pepper' stored passwords

	< /dev/urandom tr -dc A-Z-a-z-0-9 | head -c${1:-96};echo;

Add both hash values to the configuration file, and set the http_bind_address to the public ip of the server

	sudo nano /etc/graylog/server/server.conf

	root_password_sha2 = {SHA256 HASH OF ADMIN PASSWORD}
	password_secret = {GENERATED SECRET}
	http_bind_address = {HOST IP}

# Start Graylog and enable the service on boot

Reload the system to see graylog service

	sudo systemctl daemon-reload

Enalbe the service to start on boot

	sudo systemctl enable graylog-server.service

Start graylog

	sudo systemctl start graylog-server.service

Check the status of graylog

	sudo systemctl status graylog-server.service
