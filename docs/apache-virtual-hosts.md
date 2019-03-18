---
id: apache-virtual-hosts
title: Setup Apache Virtual Hosts
---

Apache Virtual Hosts allows you to run more than one website on a single machine. With Virtual Hosts, you can specify the site document root (the directory which contains the website files), create a separate security policy for each site, use different SSL certificates and much more.

## Add apache vhost modules

```shell
a2enmod rewrite
sudo a2enmod vhost_alias
```

Activate the new modules by restarting apache

```shell
sudo service apache2 restart
```

## Create a Virtual Hosts

By default on Ubuntu systems, Apache Virtual Hosts configuration files are stored in `/etc/apache2/conf-available` directory and can be enabled by creating symbolic links to the `/etc/apache2/conf-enabled` directory

Add a new conf file

```shell
sudo nano /etc/apache2/conf-available/vhost.conf

DocumentRoot "/var/www"

<VirtualHost *:80>
    ServerName randomapples.be
    ServerAlias www.randomapples.be
    VirtualDocumentRoot /var/www/html
RewriteEngine on
RewriteCond %{SERVER_NAME} =www.randomapples.be [OR]
RewriteCond %{SERVER_NAME} =randomapples.be
RewriteRule ^ https://%{SERVER_NAME}%{REQUEST_URI} [END,NE,R=permanent]
</VirtualHost>

<Directory "/var/www">
    Options FollowSymLinks MultiViews
    AllowOverride All
    Order allow,deny
    Allow from all
</Directory>

<VirtualHost *:80>
    ServerName staging.randomapples.be
    ServerAlias *.randomapples.be
    VirtualDocumentRoot /var/www/%1
    Alias /robots.txt /var/www/robots.txt
</VirtualHost>
```

Create a symlink as shown below to activate this conf file in apache `/etc/apache2/conf-enabled`

```shell
sudo ln -s /etc/apache2/conf-available/vhost.conf /etc/apache2/conf-enabled/
```

Once the configuration is enabled test if the syntax is correct with

```shell
sudo apachectl configtest
```

In order for the changes to take effect, restart the apache2 service.

```shell
sudo service apache2 restart
```