# Radiator OpenRoaming configuration repository

This is a repository for Radiator OpenRoaming related configurations, configuration guides and supporting code. Here you will find all things needed to configure Radiator for OpenRoaming.


## Installing on Ubuntu server

### Install Radiator and supporting packages:

sudo apt-get install radiator radiator-radius-utilxs libcache-fastmmap-perl libnet-dns-perl

### Ensure that example template configuration is disabled and Radiator stopped: 

sudo systemctl stop radiator
sudo systemctl disable radiator

### Enable Radiator OpenRoaming instances:

sudo systemctl enable radiator@radius_proxy_auth radiator@radius_proxy_acct radiator@radsec_inbound_local_clients radiator@radsec_inbound_openroaming radiator@radsec_outbound_openroaming radiator-instances

### Start all enabled Radiator OpenRoaming instances:

sudo systemctl restart radiator-instances

