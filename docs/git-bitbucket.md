---
id: git-bitbucket
title: Git & Bitbucket
---

Version control systems are increasingly indispensable in modern software development as versioning allows you to keep track of your software at the source level. You can track changes, revert to previous stages, and branch to create alternate versions of files and directories.

One of the most popular version control systems currently available is Git. Many projects’ files are maintained in a Git repository, and sites like GitHub, GitLab, and Bitbucket help to facilitate software development project sharing and collaboration.

In this guide, we will demonstrate how to install and configure Git on an Ubuntu 18.04 server.

## Install

First, use the apt package management tools to update your local package index. With the update complete, you can download and install Git:

```shell
sudo apt update
sudo apt install git
```

You can confirm that you have installed Git correctly by running the following command:

```shell
git --version
```
```shell
Output
git version 2.17.1
```


With Git successfully installed, you can now move on to the Setting Up Git section of this tutorial to complete your setup.

## Setup Git

Now that you have Git installed, you should configure it so that the generated commit messages will contain your correct information.

This can be achieved by using the `git config` command. Specifically, we need to provide our name and email address because Git embeds this information into each commit we do. We can go ahead and add this information by typing:

```
git config --global user.name "Your Name"
git config --global user.email "youremail@domain.com"
```

We can see all of the configuration items that have been set by typing:

```
git config --list
```
```
Output
user.name=Your Name
user.email=youremail@domain.com
...
```
The information you enter is stored in your Git configuration file, which you can optionally edit by hand with a text editor like this:

```
nano ~/.gitconfig
```
```
[user]
  name = Your Name
  email = youremail@domain.com
```

There are many other options that you can set, but these are the two essential ones needed. If you skip this step, you’ll likely see warnings when you commit to Git. This makes more work for you because you will then have to revise the commits you have done with the corrected information.

## SSH key

SSH, or secure shell, is an encrypted protocol used to administer and communicate with servers. When working with an Ubuntu server, chances are you will spend most of your time in a terminal session connected to your server through SSH.

In this guide, we'll focus on setting up SSH keys for a Ubuntu server. SSH keys provide an easy, secure way of logging into your server and are recommended for all users.


```shell
sudo su 
cd /root/.ssh/
ssh-keygen -t rsa -C "yourname@yourdomain.com"
cat id_rsa.pub
```