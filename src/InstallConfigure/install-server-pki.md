# Installation and Configuration: Server PKI #

## Software Versions ##
* N/A

## Prerequisites ##

* Signed PKI certificate and associated private key are available.
* Java JDK is installed (need keytool)
* OpenSSL is installed

## Outcome ##

* Server PKI certificates and private keys will be installed securely on the
server
* Server certificates are packaged in Java Keystore and PKCS12 format

## Overview ##

1. Install signed certificate and private key
2. Create Keystores

## Assumptions ##
* None

## Procedure ##

1. Install signed certificate and private key

		install -o root -g root -m 444 <path-to-signed-certificate>.crt /etc/pki/tls/certs/$(hostname --fqdn).crt
		install -o root -g root -m 400 <path-to-private-key>.key /etc/pki/tls/private/$(hostname --fqdn).key
		
2. Create Keystores
* Create Keystore directory

		install -o root -g root -m 750 -d /etc/pki/keystores

* Create PKCS12 Keystore

		cd /etc/pki/keystores
		openssl pkcs12 -export -out $(hostname --fqdn).p12 -in /etc/pki/tls/certs/$(hostname --fqdn).crt -inkey /etc/pki/tls/certs/$(hostname --fqdn).key --name $(hostname --fqdn)

	_NOTE: provide a password for the private key, and then again for the PKCS12 keystore_ 

* Create Java Keystore

		cd /etc/pki/keystores
		keytool -importkeystore -srckeystore /etc/pki/tls/keystores/$(hostname --fqdn).p12 -srcstoretype PKCS12 -destkeystore $(hostname --fqdn).jks -deststoretype JKS
		
	_NOTE: provide a password for the private key, and then again for the JKS_ 
		

