# How To Set Up a Firewall on Ubuntu 22.04 to allow SSH

UFW, or Uncomplicated Firewall, is a simplified firewall management interface that hides the complexity of lower-level packet filtering technologies such as `iptables` and `nftables`. This is the best choice for configuring a firewall for the environment built in [Graylog 5.1](https://github.com/hoskey76/Linux/tree/main/Graylog%205.1)

This tutorial will show you how to set up a simple firewall with UFW on Ubuntu 22.04 to allow SSH connections.

# Allowing SSH Connections
To configure your server to allow incoming SSH connections:

    sudo ufw allow ssh

This will create a firewall rule that allows all connections on port `22`

You can also specifiy the port as a rule instead of the service:

    sudo ufw allow 22
    sudo ufw allow 2222

# Enabling UFW

To enable UFW:

    sudo ufw enable

Note: You can enable other services, ports, or even a range of ports in a similar fashion, which I will visit in future projects.
