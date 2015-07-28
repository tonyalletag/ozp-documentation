# Installation and Configuration: OZP IWC and Client Components #

## Software Versions ##
* Apache HTTPD 2.2/OpenResty 1.7.0
* OZP WebTop 1.0
* OZP Center 1.0
* OZP HUD 1.0
* OZP IWC 1.0


## Prerequisites ##
* Either NGINX/OpenResty (secured) or Apache HTTPD (secured) is installed on the server
* URL for
  * OZP primary entrypoint
  * OZP Rest backend
  * OZP Metrics Service

## Outcome ##

The OZP IWC, WebTop, Center, and HUD mobile code will be available for clients to download
and run from a secure Apache HTTD instance.  Client X.509 certificates will be required
to access and run the mobile code components.

## Notes ##
* It is expected that the URL used for all of the services is an alias (CNAME) that points to a
loadbalancer.

## Overview ##

1. Create OZP Primary Configuration
2. Install IWC
2. Install OZP WebTop, Center, and HUD
3. Configure IWC
3. Configure OZP WebTop, Center, and HUD


## Assumptions ##
* CentOS 6 using YUM for package management
* IWC and Client Components are the packaged distributions (tar/gzipped), not a copy of the source code

## Notes about Procedure ##
The procedure below uses the following variables:
| Variable          | Represents                       | Example                              |
|-------------------|----------------------------------|--------------------------------------|
| @PRIMARY_URL@     | Primary URL to service           | https://ozp.example.com              |
| @BACKEND_URL@     | Root URL for OZP-Rest            | https://ozp.example.com/rest         |
| @METRICS_URL@     | URL for Metrics service          | https://metrics.example.com          |
| @FEEDBACK_EMAIL@  | Email address for user feedback  | ozp-support@ozp.example.com          |
| @DEV_URL@         | URL to development info/resources| https://ozp-dev.example.com          |


## Procedure ##

1. Create OZP Primary Configuration
    1. Create a file named `OzoneConfig.js` with the following content:
      `window.OzoneConfig = {
            'API_URL': '@BACKEND_URL@/api',
            'CENTER_URL': '@PRIMARY_URL@/center',
            'DEVELOPER_RESOURCES_URL': '@DEV_URL@',
            'FEEDBACK_ADDRESS': '@FEEDBACK_EMAIL@',
            'HUD_URL': '@PRIMARY_URL@/hud',
            'IWC_URL': '@PRIMARY_URL@/iwc',
            'METRICS_URL': '@METRICS_URL@',
            'WEBTOP_URL': '@PRIMARY_URL@/webtop',
            'HELP_DOCS': {
                'Help doc 1': '/path/to/document',
                'Help doc 2': '/path/to/document',
                'Help doc 3': '/path/to/document',
                'Help doc 4': '/path/to/document',
                'Help doc 5': '/path/to/document'
            },
            'HELP_VIDEOS': {
                'Video 1': '/path/to/video',
                'Video 2': '/path/to/video',
                'Video 3': '/path/to/video',
                'Video 4': '/path/to/video',
                'Video 5': '/path/to/video'
            }
        };`
    
2. Install IWC
   1. Extract IWC archive to temporary location
      `cd /tmp`
      `mkdir iwc-release`
      `cd iwc-release && tar zxf <path-to-iwc-archive>`
  2. Create the IWC entrypoint under the web server document root
     `install -m 755 -o root -g root -d /data/www/html/iwc`
  3. Move the contents of the dist subfolder to the IWC entrypoint
     `cp -r /tmp/iwc-release/dist/* /data/www/html/iwc/`
3. Install OZP WebTop, Center, and HUD
   1. Create the WebTop, Center, and HUD entrypoints under the web server document root
      `install -m 755 -o root -g root -d /data/www/html/{webtop,hud,center}`
   2. Extract the 
