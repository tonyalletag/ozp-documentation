# Installation and Configuration: MariaDB #

## Software Versions ##
* MariaDB 5.5.42

## Prerequisites ##

* Available Large disk storage area (>10GB)
* YUM repo with MariaDB RPM
    * Where internet access is available, install the yum repo per the instructions at https://mariadb.com/kb/en/mariadb/yum/
    * For private networks, the following MariaDB RPMs should be downloaded, and loaded into a custom YUM repo
        * MariaDB-5.5.42-centos6-x86_64-client.rpm
        * MariaDB-5.5.42-centos6-x86_64-common.rpm
        * MariaDB-5.5.42-centos6-x86_64-compat.rpm
        * MariaDB-5.5.42-centos6-x86_64-server.rpm
        * MariaDB-5.5.42-centos6-x86_64-shared.rpm
* Root DB user password  
	
## Overview ##

1. Install Package from YUM repo
2. Create Data Storage Folder
3. Configure MariaDB
4. Configure Firewall to allow ingress
5. Configure and Start Service
6. Initialize Database Server
7. Verify MariaDB


## Assumptions ##
* /data is already created with 755 root:root permissions, on a partition with >10GB free space

## Procedure ##

1. Install Package from YUM repo

	`yum install MariaDB-server`

2. Create Data Storage Folder

	`install -o mysql -g mysql -m 775 -d /data/dbs`

3. Configure MariaDB
    * Update /etc/my.cnf with the following entries:
      `[server]`
      `datadir=/data/dbs`


4. Configure Firewall to allow ingress to node
    * Add the following line to `/etc/sysconfig/iptables` before any REJECT rules

			`A INPUT -m state --state NEW -m tcp -p tcp --dport 3306 -j ACCEPT`

    * Reload the rules:

            `service iptables reload`

5. Configure and Start Service

	`chkconfig mysql on`
	`service mysql start`

_NOTE: if you receive a failure when starting, you will need to check
permissions on the data directory (/data/dbs) to ensure the
`mysql` user can read and write to it._

6. Initialize Database Server

    `sudo -l -u mysql mysql_install_db --datadir=/data/dbs`

6. Verify MariaDB

	<TBD>

		

