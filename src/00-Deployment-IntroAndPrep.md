# Standard OZP (Next Generation) Deployment #

## Scope ##
This document covers operational system deployment plans, including
systems intended for pre-production testing (e.g. Integration/Staging/QA systems).

## Overview ##
This document provides details on the intended deployment for OZP within an Enterprise
network.

This information will be useful for the Systems/Network Administration and Operations
teams, but will also be useful for system developers when troubleshooting, or when
identifying usage scenarios for feature development.


## Document Layout ##

The document will be ordered as:

* Deployment Goals
* Deployment Architecture 
* Platform Stack
* Component-Platform Mapping
* Deploying OZP

## Deployment Goals ##
* 99.99% Availability
* Minimal Performance Degradation under high load
* Efficient use of the utility resources (minimization of operational costs)
* Capable of handling > 25,000 users simlutaneously

## Deployment Architecture ##

The architecture chosen to acheive the above stated goals was a multi-tiered, highly modular architecture, consisting of:

* Tier 0: Load Balancing
* Tier 1: Web Content Distribution
    * Module A: Static Web Content Distribution
    * Module B: Dynamic Web Content Distribution
* Tier 2: Middleware Analysis & Processing
* Tier 3: Data Persistence
    * Relational Data (RDBMS)

## Platform Stack ##

### General ###

OZONE uses an integration of multiple "stacks" - a collection of independently managed software components, integrated together to form a single, logical service - instead of a single platform stack

Each stack consist of a common base:
* CentOS 6 x86-64

### Non-developed Software Used ###
The following set of (non-developed) software was used to create the stacks associated with each
tier.  

* OpenResty/NGINX 1.7.10
* Apache HTTPD 2.4
* Java Development Kit 1.7
    * Note: Preference is for Oracle/Sun's JDK, but OpenJDK will also work
* Apache Tomcat 7.0
* HAProxy 1.5
* MariaDB 5.5
* PHP 5.6

### Tier Platform Stacks ###

The stacks listed below use the non-versioned name of software.  Refer to the above list for specific version information.

| User Service     | Metrics      |
===================================
| HAProxy          | HAProxy      |
| NGINX            | Apache HTTPD |
| Apache Tomcat    | PHP          |
| MariaDB          | MariaDB      |


#### Tier 0: Load balancing ####
This tier uses the following platform stack:
* CentOS
* HAProxy

#### Tier 1: Web Content Distribution ####

##### Module A: Static Web Content Distribution #####
* CentOS
* OpenResty (NGINX)

##### Module B: Dynamic Web Content Distribution #####
* CentOS
* Apache HTTPD

#### Tier 2: Middleware Analysis & Processing ####
* CentOS
* Apache Tomcat

#### Tier 3: Data Persistence ####
* CentOS
* MariaDB


## Deploying OZP ##

### Non-Developed Software Preparation ###

No preparation beyond acquisition through normal methods is required.

### Developed Software Preparation ###

In order to maximize the reach of the software, the OZP-Rest component is released using the default security module which does not
provide X.509v3 based PKI authentication and authorization.  If this feature is to be used, the OZP-Rest component must be customized.

#### Additional Software Required ####
* [OZP Security Plugin](https://github.com/ozone-development/ozp-security)
    * This plugin will need to be built prior to customization.  The resulting JAR file will be used in the customization steps.

#### Customization Procedure ####
[Customizing OZP Rest for X.509v3 PKI](BuildCustomize/customizing-ozp-rest-war.md)

### 




