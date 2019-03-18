---
id: apache-letsencrypt
title: SSL with Let's Encrypt
---

Let's Encrypt is a Certificate Authority (CA) that provides an easy way to obtain and install free TLS/SSL certificates, thereby enabling encrypted HTTPS on web servers. It simplifies the process by providing a software client, Certbot, that attempts to automate most (if not all) of the required steps. Currently, the entire process of obtaining and installing a certificate is fully automated on both Apache and Nginx.

## Prerequisites

- A fully registered domain name. This tutorial will use example.com throughout. You can purchase a domain name on [Namecheap](https://namecheap.com/), get one for free on [Freenom](http://www.freenom.com/en/index.html), or use the domain registrar of your choice.

- Both of the following DNS records set up for your server.
    - An A record with example.com pointing to your server's public IP address.
    - An A record with www.example.com pointing to your server's public IP address.


## Install Certbot & obtain an SSL certificate

The first step to using Let's Encrypt to obtain an SSL certificate is to install the Certbot software on your server.

```shell
sudo apt install python-certbot-apache
```

Certbot provides a variety of ways to obtain SSL certificates through plugins. The Apache plugin will take care of reconfiguring Apache and reloading the config whenever necessary. To use this plugin, type the following:

```shell
sudo certbot --apache -d example.com -d www.example.com
```

This runs certbot with the --apache plugin, using -d to specify the names you'd like the certificate to be valid for.

If this is your first time running certbot, you will be prompted to enter an email address and agree to the terms of service. After doing so, certbot will communicate with the Let's Encrypt server, then run a challenge to verify that you control the domain you're requesting a certificate for.

## Possible errors

```
 - Could not be found to be deleted /etc/apache2/conf-available/vhost-le-ssl.conf - Certbot probably shut down unexpectedly
```

The problem seems to be a RewriteCond without (commented out) a following RewriteRule.
We'll need to improve our mod_rewrite rule parsing methods to fix this bug, so I'll change title of this issue accordingly to keep track of the process.

To work around this, you can comment out the orphaned RewriteCond line from your configuration file. Sorry for the inconvinience.

```
<VirtualHost *:80>
    ...
RewriteEngine on
RewriteCond %{SERVER_NAME} =domain.com
#RewriteRule ^ https://%{SERVER_NAME}%{REQUEST_URI} [END,QSA,R=permanent]
</VirtualHost>
```

![Github Issue FIXED](https://github.com/certbot/certbot/issues/5255#issuecomment-346183223)