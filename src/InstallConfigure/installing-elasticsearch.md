# Installation and Configuration: ElasticSearch Node #

## Prerequisites ##

* Java JDK Installed on system
    * Oracle JDK is recommended; OpenJDK 1.7+ is sufficient
* Available Large disk storage area (>10GB)
* YUM repo with ElasticSearch RPM
    * Where internet access is available, the main ElasticSearch repo can be used per these instructions: http://www.elastic.co/guide/en/elasticsearch/reference/1.4/setup-repositories.html#_yum
	* For private networks, the ElasticSearch RPM should be downloaded, and loaded into a custom YUM repo
	
## Overview ##

1. Install Package from YUM repo
2. Create Data Storage Folder
3. Configure ElasticSearch
4. Configure Firewall to allow ingress to node
5. Configure and Start Service
6. Verify ElasticSearch Service is Running


## Assumptions ##
* The /data partition is pre-created on a disk with >10G available

## Notes and Considerations ##
This particular ElasticSearch deployment uses unicast "discovery" - all nodes must be listed prior to starting the cluster.  This method should be usable across
all infrastructure platforms, including those that disable multicast (the default discovery method of ElasticSearch).

Use of the multicast/dynamic discovery capabilities is up to the deployer, but will not be described here.

## Procedure ##

1. Install Package from YUM repo

	`yum install elasticsearch`

2. Create Data Storage Folder

	`install -o elasticsearch -g elasticsearch -m 775 -d /data/es`

3. Configure ElasticSearch
    * Search for the associated keys (text before the ':'), uncomment if necessary, and update with the associated value, substituting locally relevant values for anything between angle brackets:
    	* `cluster.name: ozp-<dev|int|prod>-search`
    	* `index.number_of_replicas: 2`
		* `path.data: /data/es`
		* `path.work: /var/lib/elasticsearch`
		* `path.logs: /var/log/elasticsearch`
		* `bootstrap.mlockall: true`
		* `discovery.zen.ping.multicast.enabled: false`
		* `discovery.zen.ping.unicast.hosts: ["<es-node1", "<es-node2>"]`
        	* _Note: List all nodes here, including the one you are configuring_

4. Configure Firewall to allow ingress to node
    * Add the following line to `/etc/sysconfig/iptables` before any REJECT rules

			`A INPUT -m state --state NEW -m tcp -p tcp --dport 9200:9400 -j ACCEPT`
	* Reload the rules:

			`service iptables reload`
	
5. Configure and Start Service

	`chkconfig elasticsearch on`
	`service elasticsearch start`

6. Verify elasticsearch service

	`curl http://localhost:9200/_cluster/health`

		

