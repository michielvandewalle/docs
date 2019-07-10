---
id: commandline
title: Commandline
---

# Tar

A Linux tarball (“tar.gz” or “tar.bz2” file ) is nothing but a system file format that combines and compresses multiple files. 

```
# Create an archive out of specified files
tar -zcvf archive.tar.gz index.html hello.html
tar -zcvf archive.tar.gz mysite/
tar -zcvf --exclude-vcs --exclude=/vendor archive.tar.gz dir/

# Extracting
tar -zxvf archive.tar.gz
tar -zxvf archive.tar.gz index.html
tar -zxvf archive.tar.gz mysite/
```

# Symlink

The ln command is used to create symlinks or symbolic links. All this does is create a shortcut to another location which can be used to access certain files in a location that is not your project source.

```
# Basic format
ln -s <where do I point to> <what will I call it>

#  Create a new symlink to the public directory in Craft
ln -s ~/Sites/craftrepo/public www

# Afraid you'll overwrite an existing symlink? Make a back-up automatically!
ln -s -b ~/Sites/craftrepo/public www

# Or ask to be prompted
ln -s -i ~/Sites/craftrepo/public www

# Or just force it if you don't care
ln -s -f ~/Sites/craftrepo/public www
```

# SHH

Whenever you're working on a remote server, you should prefer to do so using its own commandline just like you would use Terminal locally. The easiest way to do this is by connecting remotely to the server using the ssh command. In order for you to be able to connect, the server needs to know your public key.

```
# View the ssh key on any UNIX environment
cat < ~/.ssh/id_rsa.pub

# Or, since you're on a Mac
pbcopy < ~/.ssh/id_rsa.pub
```

Make a connection

```
# Connect to a remote server with user root via domain name
ssh root@domain.com

# Connect via ip
ssh root@192.168.0.1

# Connect using a different port
ssh -p 1337 root@domain.com

# Enable debugging
ssh -d root@domain.com
```


# SCP

The scp command allows you to copy files and/or directories securely over ssh! This is handy for syncing but keep in mind it overwrites destination files. The syntax of this command is fairly straightforward with 1 caveat when it comes to directories. Below are a few examples of situations you'll likely be using scp in.


## Upload

```
# Upload a local file
scp index.php root@domain.com:/var/www/index.html

# Upload a local directory
scp -r /var/www/uploads/ root@domain.com:/var/www/
```

## Download

```
# Download a remote file
scp root@domain.com:/var/www/about.html ~/Desktop/about.html

# Download a remote directory
scp -r root@domain.com:/var/www/uploads /var/www
```


# CHMOD

To modify these permissions to (dis)allow more actions one would use the chmod command. If we were to change the permission set for the above file to allow read (4 or r), write (2 or w) and execute (1 or x) access for the owner and the group ownerships, while limiting everyone else to just getting read and execute access, we would use the first command below. If we were to apply this to the site directory, we would use the second command which adds an optional -r parameter to recursively apply the same permission to each underlying directory and file. If we just want to do it on the directory but not its child directories and files we'd leave out the -r parameter.

```
# Apply read, write and execute access for owner and group, read and execute access for everyone else
chmod 775 data.json

# The same as the above but recursively on the given directory
chmod -R 775 site

# The same as the above but just on the given directory, not its child directories and files
chmod 775 site
```


# CHOWN

On Unix-like operating systems, the chown command changes ownership of files and directories in a filesystem.

# Change the owner of the given file to kosmonaut
chown kosmonaut data.json

# For directories we can apply the optional -R again
chown -R kosmonaut site
