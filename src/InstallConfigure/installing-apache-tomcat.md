# Installation and Configuration: Apache Tomcat #

## Software Versions ##
* Apache Tomcat 7.0.33

## Prerequisites ##

* YUM repo with Apache Tomcat RPM
    * Use the standard (pre-loaded) and EPEL repositories (`yum install -y epel-release`)
* PKI/SSL Certificates Keystore and Trust Store ready and available


## Outcome ##

Apache Tomcat will be running on the local system on port 8443 and 8009.  Connections
on port 8443 will be encrypted using SSL, and 8009 connections will use the Apache AJP
communications protocol.

No web apps will be started or running.

## Overview ##

1. Install Package from YUM repo
2. Configure Tomcat Connector (`/usr/share/tomcat/conf/server.xml`)
3. Configure JVM options for SSL and Heap Memory (`/etc/sysconfig/tomcat`)
4. Configure Firewall to allow ingress
5. Configure and Start Service


## Assumptions ##
* Keystore is stored in `/etc/pki/tls/keystores/<fqdn>.jks` and is password protected
    * Where `<fqdn>` is the fully-qualified domain name of the local host (e.g. appsvr01.example.com).

* Trust Store is stored at `/etc/pki/CA/ozp-trusts.jks` and is password protected
	

## Procedure ##

1. Install Package from YUM repo

		yum install -y tomcat

2.  Configure Tomcat Connector (/usr/share/tomcat/conf/server.xml)
    * Under the `<Service name="Catalina">` section in `/usr/share/tomcat/conf/server.xml`, add the following entry:

    			<Connector
    				protocol="HTTP/1.1"
    				port="8443" maxThreads="200"
    				scheme="https" secure="true" SSLEnabled="true"
    				keystoreFile="/etc/pki/tls/keystores/<fqdn>.jks"
    				keystorePass="<keystore-password>"
    				truststoreFile="/etc/pki/CA/ozp-trusts.jks"
    				truststorePass="<truststore-password>"
    				clientAuth="want" sslProtocol="TLS"/>

	_NOTE 1: We need to set `clientAuth` to "want" instead of "true" due to the use of the HTTP OPTIONS request, which, by the official standard,
		   will *not* pass any kind of authentication information; it is expected to be able to anonymously request OPTIONS._

	_NOTE 2: The AJP connector is already available in the default server.xml file.  No additional configuration is required to enable it._

3. Configure JVM options for SSL and Heap Memory (`/etc/sysconfig/tomcat`)
    * Add the following entries to the `/etc/sysconfig/tomcat` file:

    			export JAVA_OPTS="${JAVA_OPTS} -Xms2g -Xmx2g -XX:MaxPermSize=512m"
    			export JAVA_OPTS="${JAVA_OPTS} -Djavax.net.ssl.keyStore=/etc/pki/tls/keystores/$(hostname --fqdn).jks"
    			export JAVA_OPTS="${JAVA_OPTS} -Djavax.net.ssl.keyStorePassword='"$(echo <keystore-password>)"'"
    			export JAVA_OPTS="${JAVA_OPTS} -Djavax.net.ssl.trustStore=/etc/pki/CA/ozp-trusts.jks"
    			export JAVA_OPTS="${JAVA_OPTS} -Djavax.net.ssl.trustStorePassword=<truststore-password>"


4. Configure Firewall to allow ingress to node
    * Add the following line to `/etc/sysconfig/iptables` before any REJECT rules

			A INPUT -m state --state NEW -m tcp -p tcp --dport 8443 -j ACCEPT
			A INPUT -m state --state NEW -m tcp -p tcp --dport 8009 -j ACCEPT

    * Reload the rules:

            service iptables reload


5. Configure and Start Service

		chkconfig tomcat on
		service tomcat start

		

