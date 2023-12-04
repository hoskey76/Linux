# Installing ElasticSearch 7.10.2


1.Download and install the public signing key

	wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -

2.Install the APT-https package

	sudo apt-get install apt-transport-https

3.Save the repository definition to file

	echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-7.x.list

4.Install elasticsearch

	sudo apt-get update && sudo apt-get install elasticsearch

5.Modify the configuration file, set the cluster name, and enable index creation

	sudo tee -a /etc/elasticsearch/elasticsearch.yml > /dev/null <<EOT
	cluster.name: graylog
	action.auto_create_index: false
	EOT

6.Reload system

	sudo systemctl daemon-reload

7.Enable elasticsearch on boot

	sudo systemctl enable elasticsearch.service

8.Restart elasticsearch

	sudo systemctl restart elasticsearch.service

9.Verify elasticsearch is running

	sudo systemctl status elasticsearch.service
