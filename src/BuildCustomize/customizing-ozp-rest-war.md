# Customizations: OZP Rest WAR #

## Overview ##

The OZP Rest WAR is delivered as a FOSS, general purpose web application supporting basic authentication.
For Enterprise deployments using X.509v3 PKI, the web app requires additional customizations.
policies.

## Resources ##
* OZP IAA Plugin
    * Source Code Repository: https://github.com/ozone-development/ozp-security
* Apache HTTPClient 4.3 Java JARs
    * http://archive.apache.org/dist/httpcomponents/httpclient/binary/httpcomponents-client-4.3.6-bin.tar.gz

## Prerequisites ##
* Installed Software:
    * unzip
    * java 1.7+ (OpenJDK is fine)
* ozone-security-*.jar built from sources
    
## Customization Overview ##
1. Explode WAR file to temporary directory
2. Replace security plugin with OZP IAA Plugin
3. Add Apache HTTPClient JARs
4. Repack WAR


## Customization Process ##
1. Explode WAR file to temporary directory

        mkdir /tmp/extract/ozp-rest
        cd /tmp/extract/ozp-rest
		unzip <path-to-ozp-rest-war>

2. Replace security plugin with OZP IAA Plugin

		cd /tmp/extract/ozp-rest
		rm -f WEB-INF/lib/ozone-security*.jar
        cp <path-to-ozp-security-repo>/target/ozone-security*.jar WEB-INF/lib/

3. Add HTTPClient JARs

		tar -C WEB-INF/lib --strip-components=2 -zxf <path-to-httpclient-archive>/httpcomponents-client-4.3.6-bin.tar.gz \
	        httpcomponents-client-4.3.6/lib/httpclient-4.3.6.jar \
			httpcomponents-client-4.3.6/lib/httpmime-4.3.6.jar \
			httpcomponents-client-4.3.6/lib/fluent-hc-4.3.6.jar \
			httpcomponents-client-4.3.6/lib/httpclient-cache-4.3.6.jar \
			httpcomponents-client-4.3.6/lib/httpcore-4.3.3.jar \
			httpcomponents-client-4.3.6/lib/commons-logging-1.1.3.jar \
			httpcomponents-client-4.3.6/lib/commons-codec-1.6.jar

		
4. Repack WAR

        jar cf /tmp/extract/ozp-rest-x509.war *


