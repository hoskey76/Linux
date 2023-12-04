# Installing MongoDB 6.0
Installing specific release 6.0 because thats what we defined for Graylog,
official documentation: https://www.mongodb.com/docs/v6.0/tutorial/install-mongodb-on-ubuntu/

Install gnupg and curl if not already available

	sudo apt-get install gnupg curl

Import the MongoDB public GPG key

	curl -fsSL https://pgp.mongodb.com/server-6.0.asc | \
   	sudo gpg -o /usr/share/keyrings/mongodb-server-6.0.gpg \
   	--dearmor

Create list file for MongoDB

   	echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-6.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list

Reload locale package database

	sudo apt-get update

Install specific version of MongoDB, in this case 6.0

	sudo apt-get install -y mongodb-org=6.0.10 mongodb-org-database=6.0.10 mongodb-org-server=6.0.10 mongodb-org-mongos=6.0.10 mongodb-org-tools=6.0.10

OPTIONAL: Pin packages at the currently installed version to prevent unintended upgrades

	echo "mongodb-org hold" | sudo dpkg --set-selections
	echo "mongodb-org-database hold" | sudo dpkg --set-selections
	echo "mongodb-org-server hold" | sudo dpkg --set-selections
	echo "mongodb-mongosh hold" | sudo dpkg --set-selections
	echo "mongodb-org-mongos hold" | sudo dpkg --set-selections
	echo "mongodb-org-tools hold" | sudo dpkg --set-selections
			
The next steps to run MongoDB assume that you installed from the mongod-org package and not from the mongod package from Ubuntu.

When you install via the package manager the data and log directories are created during the install of MongoDB. The package install also includes the configuration file and it takes effect on startup, so if you make changes to the configuration you will need to restart the instance for the changes to take effect.

# Running MongoDB

Reload system manager configuration

	sudo systemctl daemon-reload

Start the mongod process

	sudo systemctl start mongod
	
Verify the process started

	sudo systemctl status mongod

Ensure that MongoDB will start on a system reboot

	sudo systemctl enable mongod

Other useful commands to know

	#stop MongoDB
		sudo systemctl stop mongod

	#restart MongoDB
		sudo systemctl restart mongod

	#connect to a session on the same host machine
		mongosh

# Disable Transparent Huge pages (THP)

The graylog documentation mentions that the MongoDB documentation recommends you disable Transparent Huge Pages (THP).

From the MongoDB documentation on THP:

Transparent Huge Pages (THP) is a Linux memory management system that reduces the overhead of Translation Lookaside Buffer (TLB) lookups on machines with large amounts of memory by using larger 
memory pages. However, database workloads often perform poorly with THP enabled, because they tend to have sparse rather than contiguous memory access patterns. When running MongoDB on Linux, 
THP should be disabled for best performance.

Service File - Create the systemd unit file at this location
 
	/etc/systemd/system/disable-transparent-huge-pages.service

File Contents

			[Unit]
			Description=Disable Transparent Huge Pages (THP)
			DefaultDependencies=no
			After=sysinit.target local-fs.target
			Before=mongod.service

			[Service]
			Type=oneshot
			ExecStart=/bin/sh -c 'echo never | tee /sys/kernel/mm/transparent_hugepage/enabled > /dev/null'

			[Install]
			WantedBy=basic.target

Reload systemd unit files to make the service file available for use

	sudo systemctl daemon-reload

Start the service

	sudo systemctl start disable-transparent-huge-pages

Verify THP has been set to ```[never]```

	cat /sys/kernel/mm/transparent_hugepage/enabled

Configure OS to run the service on boot

	sudo systemctl enable disable-transparent-huge-pages
