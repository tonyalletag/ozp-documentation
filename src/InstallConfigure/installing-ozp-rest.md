# Installation & Configuration: ozp-rest #

## Prerequisites ##
* Customized ozp-rest WAR
* PKI certificates for SSL
* ElasticSearch Cluster deployed and accessible from ozp-rest host
* MariaDB setup with
    * empty database `ozp` created
    * User `ozpuser` created with full privileges on `ozp` database, and access allowed from OZP-Rest host(s)
    * You have the connection information for `ozpuser`
* Access to MariaDB client `mysql` (from a separate host is preferred).
	
## Resources ##

* Database Schema @ <TBD>
* Initial Database data @ <TBD>

## Deployment Overview ##

1. Install Apache Tomcat
2. Install OZP server-level configuration files
3. Configure SSL on Tomcat
4. Load Schema into MariaDB
5. Load initial data into MariaDB
6. Install (deploy) the customized ozp-rest WAR
7. Start Tomcat


## Deployment Detailed Instructions ##

Notes:
 - Perform all steps as root
 - For CentOS 6, the <tomcat-root> directory is normally located at /usr/share/tomcat


1. Install Apache Tomcat

	`yum install -y tomcat`

2. Install OZP server-level configuration files
    * <tomcat-root>/lib/SecurityConfig.properties
	* <tomcat-root>/lib/SecurityContext.xml
	* <tomcat-root>/lib/MarketplaceConfig.groovy
	
3. Configure SSL on Tomcat

    * Configure the JVM to use the PKI certificates 
	  * This requires setting the `javax.net.ssl.*` properties, which will make
		the client certificate accessible to web apps
	  * This allows the web app to call out to PKI authenticated services
	* Configure the HTTPS connector in server.xml
	  * This provides the ingress encryption (Client -> Tomcat server)

4. Load Schema into MariaDB

	`mysql -u ozpuser -h <mariadb-host> -D ozp < <DB-Schema-File>`


5. Load Initial Data into MariaDB

	`mysql -u ozpuser -h <mariadb-host> -D ozp < <DB-Data-File>`

6. Install (deploy) the customized ozp-rest WAR
    * From a web location

		`wget -O <tomcat-root>/webapps/ozp-rest.war http://infra-adm01/cfgmgmt/apps/ozp-rest/war-latest`
        `chown tomcat:tomcat <tomcat-root>/webapps/ozp-rest.war`
    * From a local download of the WAR

        `install -o tomcat -g tomcat -m 644 <local-path>/ozp-rest.war <tomcat-root>/webapps/ozp-rest.war`

7. Start Tomcat and enable auto-start

        chkconfig tomcat on
        service tomcat start

8. Check for errors and attempt to resolve

    * Review the following logs in <tomcat-root>/logs/
        * `catalina.out`
	    * `marketplace.log`
		* `stacktrace.log`
		* `localhost.<date>.log`

