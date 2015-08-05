# Deployment Overview #

## Hardware & Network Requirements ##

### Hardware ###

The Apps Mall OZP Next Generation Platform deployment has the following hardware requirements:

* Backend Apache Tomcat (JEE) Component
    * 2 VMs with
        * 2 CPUs
		* 8GB RAM
		* 40GB Disk Space
* MariaDB (RDBMS) Component
  * 1 VM with
	* 4 CPU
	* 16GB RAM
	* 60GB Disk
* NGINX (Static Web Content Distribution) Component
  * 2 VMs with
	* 2 CPU
	* 2GB RAM
	* 60GB Disk
* NGINX (Dynamic Web) Component
  * 1 VM with
	* 2 CPU
	* 4GB RAM
	* 70 GB Disk
* HAProxy (Load Balancing) Component
  * 2 VMs with
	* 2 CPU
	* 4GB RAM
	* 40GB Disk

### Networking ###

Deployment of the OZP Next Generation platform has been tested within a private VLAN that explicitly exposes various endpoints using Firewalls and Network Address Translations (NAT).

The only requirements for Networking are:
* VLAN/private network
* Firewall with NAT support

## Security Considerations & Requirements ##

All external-faced endpoints will make use of SSL (TLSv1+).  In order to provide this function, all external IPs and Hostnames will require an associated SSL certificate from a Trusted Certificate Authority.

## Installation & Configuration Overview ##

1. [Deploy MariaDB (RDBMS)](InstallConfigure/installing-mariadb.md)
2. Deploy OZP Rest Backend
   1. [Deploy ElasticSearch Cluster](InstallConfigure/installing-elasticsearch.md)
   2. Deploy OZP Rest Web Application
       1. For Each VM running OZP Rest Web Application:
           1. [Deploy Server PKI](InstallConfigure/install-server-pki.md)
           2. [Deploy Apache Tomcat](InstallConfigure/installing-apache-tomcat.md)
           3. [Deploy OZP-REST](InstallConfigure/installing-ozp-rest.md)
3. Deploy Metrics Service
    1. [Deploy Server PKI](InstallConfigure/install-server-pki.md)
    2. [Deploy Apache HTTPD with SSL](InstallConfigure/installing-apache-httpd.md)
    3. [Deploy Metrics application](InstallConfigure/installing-metrics.md)
4. Deploy Client Application components (HUD, WebTop, Center, and IWC)
   1. For each VM hosting the client components: 
       1. [Deploy Apache HTTPD with SSL](InstallConfigure/installing-apache-httpd.md)
       2. [Deploy IWC and Client components](InstallConfigure/installing-iwc-and-client-components.md)
5. Deploy load balancers
    1. Deploy HAProxy to Static Content LB VM
    2. Configure HAProxy backends to balance over each VM in step 4.1
    3. Deploy HAProxy to Rest Backend LB VM
    4. Configure Rest Backend LB VM to balance over each VM in step 2.2.1


