---
id: server-stack
title: Server Stack
sidebar_label: Server Stack
---

## Server info

```
- LAMP
  - Ubuntu 18.04
  - Apache 2.4
  - Myslq 5.7
  - PHP 7.2
```

## SSH instellingen

Gebruik SSH om toegang te krijgen tot de server.

Controleer het path naar je SSH key
```
ssh -i ~/.ssh/id_rsa.pub ubuntu@3.120.212.138
```

**[Lees meer](https://nerderati.com/2011/03/17/simplify-your-life-with-an-ssh-config-file/)** over SSH config file


#### Toegang geven tot server

Voeg SSH key van mac toe aan de `authorized_keys` op de server.

```
# server
sudo nano  ~/.ssh/authorized_keys
```

## Werking server

De server maakt gebruik van subdomein om verschillende projecten te hosten vb. `subdomein.example.com`. Het subdomein wordt gemapt op de directories in de map `/var/www`.

```
# server
cd /var/www/
twiggy          => twiggy.coulddothiseverynight.com
twiggy-api      => twiggy-api.coulddothiseverynight.com

html            => coulddothiseverynight.com
```

De directory `html` is de main directory voor het domain.


## Project toevoegen

Projecten worden toegevoegd aan de server via `GIT`

[Lees meer](https://www.atlassian.com/git/tutorials/learn-git-with-bitbucket-cloud) over GIT met Bitbucket.

```
# server
cd /var/www/
sudo git clone git@bitbucket.org:milk-and-cookies/twiggy.git
```
Gebruik `sudo` voor de commando's

Een nieuwe project is aangemaakt op de server en in dit geval gemapt naar het domein `twiggy.coulddothiseverynight.com`

## Project updaten

Voor je begint, zorg dat de aanpassingen gepushed zijn naar Bitbucket.

```
# server
cd /var/www/twiggy/
sudo git pull origin master
```

## PHPMyAdmin

[Mysql database via PHPMyAdmin](http://coulddothiseverynight.com/phpmyadmin)

