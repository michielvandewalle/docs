---
id: apache-modules
title: Load essential apache modules
---

Apache Virtual Hosts allows you to run more than one website on a single machine. With Virtual Hosts, you can specify the site document root (the directory which contains the website files), create a separate security policy for each site, use different SSL certificates and much more.

## Headers

To enable mod headers on Apache2 (httpd) you need to run this command.
Used for `CORS` headers in `.htaccess` to work. 

```shell
sudo a2enmod headers
sudo service apache2 restart
```

Add following lines to the bottom of the `.htaccess` file to fix `Access-Control-Allow-Origin` error

```shell
<IfModule mod_headers.c>
    Header set Access-Control-Allow-Origin "*"
</IfModule>
```