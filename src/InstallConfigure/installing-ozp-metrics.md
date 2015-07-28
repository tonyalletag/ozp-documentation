# Installation & Configuration: ozp-metrics #

## Prerequisites ##
* If using CentOS/RedHat 6, you will need to locate a 3rd party PHP 5.6 YUM repository
* SSL Server Certificate (including Private Key)

## Resources ##

* PHP 5.6 for CentOS/RedHat 6.6 can be installed using the Webtatic EL repository
** Instructions at https://webtatic.com/packages/php56/
* OZP-Metrics @ <TBD>

## Assumptions ##
* The OS is CentOS 6
* EPEL repository is enabled
* The instructions below will assume the use of the Webtatic PHP 5.6 packages on CentOS 6

## Deployment Overview ##

1. Install Apache HTTPD 2.2 with mod_ssl
2. Install PHP 5.6
3. [Optional] Create a web server document root directory
4. Configure Apache HTTPD - primary config
5. Configure PHP for Apache HTTPD
6. Configure Apache HTTPD SSL
7. Deploy Metrics application to Document Root
8. Configure Metrics
9. Start Apache HTTPD
10. Verify Metrics

## Deployment Steps ##

1. Install Apache HTTPD 2.2 with mod_ssl
   `sudo yum install httpd mod_ssl`
2. Install PHP 5.6 modules
   `sudo yum install php56w php56w-cli php56w-mbstring php56w-pdo php56w-gd php56w-common php56w-mysql php56w-xml`
3. [Optional] Create a web server document root directory
   `install -m 755 -d /data`
   `install -m 755 -o apache -g apache -d /data/www/{cgi-bin,html,error,icons}`
4. Configure Apache HTTPD - primary config
   * Open /etc/httpd/conf/httpd.conf for editing
     * [If step 3 was done] Replace all instances of default document root `/var/www/html` with custom document root `/data/www/html`
     * 
     


