# Deployment Overview #

## Hardware & Network Requirements ##

### Hardware ###

The Apps Mall OZP Next Generation Platform deployment has the following hardware requirements:

* Backend Apache Tomcat (JEE) Component
    * 1 VM with
        * 2 CPUs
		* 4GB RAM
		* 40GB Disk Space
* MariaDB (RDBMS) Component
  * 1 VM with
	* 4 CPU
	* 16GB RAM
	* 60GB Disk
* MongoDB (NoSQL) Component
  * 2 VMs with
	* 4 CPU
	* 16GB RAM
	* 20GB Disk
* NGINX (Static Web Content Distribution) Component
  * 1 VM with
	* 2 CPU
	* 2GB RAM
	* 60GB Disk
* NGINX (Dynamic Web) Component
  * 1 VM with
	* 2 CPU
	* 4GB RAM
	* 70 GB Disk
* HAProxy (Load Balancing) Component
  * 1 VM with
	* 2 CPU
	* 4GB RAM
	* 40GB Disk

### Networking ###

The Apps Mall OZP Next Generation platform will be deployed to a private VLAN that will explicitly expose various endpoints using Firewalls and Network Address Translations (NAT).

The only requirements for Networking are:
* VLAN/private network
* Firewall with NAT support

## Security Considerations & Requirements ##

All external-faced endpoints will make use of SSL (TLSv1+).  In order to provide this function, all external IPs and Hostnames will require an associated SSL certificate from a Trusted Certificate Authority.

At the present time, the internal PKI service will be used to generate these certificates.  Any external clients will be required to pre-load the internal Certificate Authority (OZP-Root-CA) in order to enable a transparent trusted connection (i.e. no "untrusted connection" messages).


## Installation & Configuration Overview ##

1. Deploy OZP Rest Backend
   1. Deploy MariaDB
   2. Deploy ElasticSearch Cluster
   3. Deploy OZP Rest Web Application
2. Deploy Metrics Service
   1. Deploy MongoDB Cluster
   2. Deploy NGINX Server with PHP
   3. Deploy Metrics application (PHP-based)
3. Deploy Client Application components (HUD, WebTop, Center, and IWC)
   1. Deploy NGINX Server
   2. Deploy pre-built IWC distribution
   3. Deploy pre-built HUD distribution
   4. Deploy pre-built Center distribution
   5. Deploy pre-built WebTop distribution
