# Installation and Configuration: Apache HTTPD (secured) #

## Software Versions ##
* Apache HTTPD 2.2

## Prerequisites ##
* Server PKI is installed per [install-server-pki] instructions
* Server PKI (private key and certificate) is accessible by the apache user.
* Certificate Authority file (CA Bundle - all desired CA's listed)
* Available disk storage area is available outside of the /var partition for use as a DocumentRoot location (content, media, & data files)


## Outcome ##

Apache HTTPD is running and serving a secure virtual host on port 443 (default HTTPS port), which is configured
to only allow 2-way SSL connections.

## Overview ##

1. Install Apache HTTPD with mod_ssl
2. Create DocumentRoot directory
3. Configure HTTPD - Basics
4. Configure HTTPD - SSL
5. Configure Firewall to allow ingress to node
6. Configure and start HTTPD


## Assumptions ##
* CentOS 6 using YUM for package management
* iptables is in use and running and already configured to allow existing connections
* Available disk storage filesystem root is at /data mount point, which is owned by the `root` user.
* Certificate Authority/Trust Store is name `/etc/pki/CA/ozp-ca.crt`
* As a general process, the Private Server Key is assumed to be passphrase-less, but access controlled such that only the
  `apache` user can access it.
    * It is recommended that a stronger security mechanism is in place for production deployments - e.g. passphrase protected private key, and use
    of a mechanism to provide the passphrase to Apache HTTPD
    
	


## Procedure ##

*NOTE: substitute fully qualified domain name of host for instances of `<FQDN>` in procedures below*

1. Install Apache HTTPD with mod_ssl
   `yum install httpd mod_ssl`

2. Create DocumentRoot directory
   1. Create root directory on a filesystem separate from /var, owned by `apache` user
      `sudo install -o apache -g apache -m 0755 -d /data/www`
   2. Copy the default document root layout and files to new document root directory
      `sudo cp -pr /var/www/* /data/www`
3. Configure HTTPD - Basics
    1. Modify main HTTPD configuration file `/etc/httpd/conf/httpd.conf`:
        * Search and replace all instances of `/var/www/html` with `/data/www/html`
    
4. Configure HTTPD - SSL
   1. [Optional] Generate a private key without a passphrase for the `apache` user only:
      `openssl rsa -in /etc/pki/tls/private/<FQDN>.key -out /etc/pki/tls/private/<FQDN>.nopp.key`
      `chmod 400 /etc/pki/tls/private/<FQDN>.nopp.key`
      `setfacl -m user:apache:r /etc/pki/tls/private/<FQDN>.nopp.key`
      
   2. Modify `/etc/httpd/conf.d/ssl.conf`:
      * Set the protocol and encryption levels to high
          `SSLProtocol all -SSLv2`
          `SSLCipherSuite HIGH:!aNULL:!MD5`
      * Set the SSL certificate and Trusts:
          `SSLCertificateFile /etc/pki/tls/certs/<FQDN>.crt`
          `SSLCertificateKeyFile /etc/pki/tls/private/<FQDN>.nopp.key`
          `SSLCACertificateFile /etc/pki/CA/ozp-ca.crt`

5. Configure Firewall to allow ingress to node
    * Add the following line to `/etc/sysconfig/iptables` before any REJECT rules

	    `A INPUT -m state --state NEW -m tcp -p tcp --dport 443 -j ACCEPT`

    * Reload the rules:
        `service iptables reload`

6. Configure and Start Service
	`chkconfig httpd on`
	`service httpd start`

		

