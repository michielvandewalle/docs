---
id: ubuntu-lamp
title: Install LAMP stack
sidebar_label: LAMP stack
---

LAMP Stack is the group of services that work together in order to serve the dynamic websites and web apps. The LAMP stack can be installed and configured on almost all Linux variants such as Amazon Linux, Ubuntu, Red Hat, and Fedora.

- L = Linux as the operating system platform.
- A = Apache as the Web server
- M = MySQL as the database server
- P = PHP as the supportive language

## Install Apache

Execute the following commands to update the existing packages and install the Apache packages.

```shell
sudo apt-get update -y
sudo apt-get install apache2 -y
```

Ensure that apache service is running properly. If not, start it manually by executing the following commands:

```shell
sudo service apache2 status
sudo service apache2 restart
```

Once you have installed the Apache server and tested that the service is running properly, type the following URL on a Web browser to test that Web server is functioning properly.

`http://server-ip-address`

The default page of Apache web server should be displayed as shown in the following figure.

![default-apache](https://assets.digitalocean.com/articles/lamp_1404/default_apache.png)

## Install MySQL

MySQL is a database service that organizes and provides access to databases where your website can store information. To install MySQL on Ubuntu machine, use the following steps.

```shell
sudo apt-get install mysql-server mysql-client mysql-common php-mysql
```

The MySQL installation will begin and you would be asked to set the MySQL root password as shown in the following figure. Set the desired MySQL root password and complete the installation.

![configure-mysql-login](https://i1.wp.com/protechgurus.com/wp-content/uploads/2017/05/word-image-2.png?w=562&ssl=1)

After completing the MySQL installation, execute the following command to create database directory structure where MySQL will store its information.

```shell
sudo mysqld --initialize
```

The default installation of MySQL has some security holes. Use the following command to enhance MySQL database security.

```shell
sudo mysql_secure_installation
```

Follow the on-screen instructions, read each of the displayed settings options and understand what impact these settings will have.

```shell
Remove anonymous users? Y
Disallow root login remotely? n
Remove test database and access to it? Y
Reload privilege tables now? Y
```

Enable root login for phpMyAdmin or for external login.

```
sudo mysql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'yourpassword';
```

## Install PHP

PHP stands for Personal Home Page is a scripting language used in web designing. To install PHP and its required packages on Ubuntu, use the following command:

```
sudo apt-get install phpmyadmin php-mbstring php-gettext
```

On the Web server selection screen, select Apache2 as the web server and complete the installation.

![configure-phpmyadmin-apache](https://i1.wp.com/protechgurus.com/wp-content/uploads/2017/05/word-image-4.png?w=569&ssl=1)

On the Configuring phpmyadmin screen, click **Yes** and specify a PHPMyAdmin password.

![configure-phpmyadmin-login](https://i2.wp.com/protechgurus.com/wp-content/uploads/2017/05/word-image-5.png?w=590&ssl=1)

Now, execute the following commands to adjust the PHP settings:

```shell
sudo phpenmod mbstring
```

Finally, use the following command to restart the Apache server.

```shell
sudo service apache2 restart
```

Now, if you test your default web server page, you will get the content of the index.html file. However, we need to change it as we want to serve dynamic web pages using PHP components. To achieve this, you need to modify the dir.conf file using the following command.

```shell
sudo nano /etc/apache2/mods-enabled/dir.conf
```

In the dir.conf file, change the order of index directory by placing index.php before of all the other index files as shown in the following figure.

![configure-apache-order](https://i0.wp.com/protechgurus.com/wp-content/uploads/2017/05/word-image-6.png?w=618&ssl=1)

Now, your (L)inux (A)pache (M)ySQL and (P)HP = LAMP stack is installed and configured successfully.

## Testing

To test and validate that your Apache, MySQL, and PHP services are working properly, you can create a PHP test script under the web server directory that is /var/www/html in Ubuntu Linux. Use the following command to create the PHP test script and write the script code as given below.

```shell
sudo nano /var/www/html/test.php
<?php phpinfo(); ?>
```

Save the test.php file and restart the apache service.

```shell
sudo service apache2 restart
```

To confirm that all your configuration is done successfully, test each of LAMP service using the following URLs.

- Web server test URL: http://server-ip-address
- PHP script test URL: http://server-ip-address/test.php
- PHPMyAdmin test URL: http://server-ip-address/phpmyadmin