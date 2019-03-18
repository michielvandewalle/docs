---
id: apache-node
title: Node.js on Apache Server
---

use reverse proxy technique to make apache be able to run node.js application on the same server

## Concept

Since we cannot run the both node.js and apache to listen the same port. we’ll to config apache to act like a reverse proxy and pass the request to node.js application for a specific url. For example, if you already have Apache server running on localhost and want to run Node.js app on localhost/node, the flow should looks like this

![apache-node-concept](https://redstapler.co/wp-content/uploads/2017/12/node-apache-same-server1.png)

## Install Node & NPM

To install, run the commands below

```
sudo apt install nodejs
sudo apt install npm
```

After installing, both Node.js and NPM modules should be installed and ready to use.
You can use the commands below to view the version number installed.

```
node -v
npm -v
```

## Setup

First let’s start the node application to listen on port 3000.

Create a file package.json
```json
{
  "name": "nodejs-hello-world",
  "version": "1.0.0",
  "description": "Hello World sample app",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  }
}
```

Create a file index.js
```javascript
var http = require('http');

var server = http.createServer(function(request, response) {

    response.writeHead(200, {"Content-Type": "text/plain"});
    response.end("Hello World!");

});

var port = 3000;
server.listen(port);

console.log("Server running at http://localhost:%d", port);
```

This is the simple app to listen for http request using Express and return a simple text which you will be able to see it on browser if you navigate to localhost:3000

![node-app-hello-world](https://redstapler.co/wp-content/uploads/2017/12/node-apache-same-server2.png)

Next we will make the Apache reroute the request by using proxypass directive. Just head to httpd.conf file and add

```shell
ProxyPass /node http://localhost:3000/
```
Reroute node for a subdomain like node.yourdomainname.be, add a Virtualhost in vhost.conf

```shell
sudo nano  /etc/apache2/conf-available/vhost.conf
```
```
<VirtualHost *:80>
    ServerName node.randomapples.be
    ProxyPass / http://localhost:3000/
    ProxyPassReverse / http://localhost:3000/
</VirtualHost>
```

You can change the /node to whatever url that you want to serve your node application
Then, make sure that you have enable the mod_proxy and mod_proxy_http modules

```shell
sudo a2enmod proxy
sudo a2enmod proxy_http
sudo a2enmod proxy_balancer
sudo a2enmod lbmethod_byrequests
```

Activate the new modules by restarting apache

```
sudo service apache2 restart
```

## Install PM2

Now we will install PM2, which is a process manager for Node.js applications. PM2 provides an easy way to manage and daemonize applications (run them in the background as a service).

We will use npm, a package manager for Node modules that installs with Node.js, to install PM2 on our server. Use this command to install PM2:

```shell
sudo npm install -g pm2
```

The `-g` option tells `npm` to install the module globally, so that it's available system-wide.

### Start Application
The first thing you will want to do is use the pm2 start command to run your application, hello.js, in the background:

```shell
pm2 start hello.js
```

This also adds your application to PM2's process list, which is outputted every time you start an application:

```
Output
[PM2] Spawning PM2 daemon
[PM2] PM2 Successfully daemonized
[PM2] Starting hello.js in fork_mode (1 instance)
[PM2] Done.
┌──────────┬────┬──────┬──────┬────────┬─────────┬────────┬─────────────┬──────────┐
│ App name │ id │ mode │ pid  │ status │ restart │ uptime │ memory      │ watching │
├──────────┼────┼──────┼──────┼────────┼─────────┼────────┼─────────────┼──────────┤
│ hello    │ 0  │ fork │ 3524 │ online │ 0       │ 0s     │ 21.566 MB   │ disabled │
└──────────┴────┴──────┴──────┴────────┴─────────┴────────┴─────────────┴──────────┘
 Use `pm2 show <id|name>` to get more details about an app
 ```

 As you can see, PM2 automatically assigns an App name (based on the filename, without the .js extension) and a PM2 id. PM2 also maintains other information, such as the PID of the process, its current status, and memory usage.

Applications that are running under PM2 will be restarted automatically if the application crashes or is killed, but an additional step needs to be taken to get the application to launch on system startup (boot or reboot). Luckily, PM2 provides an easy way to do this, the startup subcommand.

The startup subcommand generates and configures a startup script to launch PM2 and its managed processes on server boots:

```shell
pm2 startup systemd
```

The last line of the resulting output will include a command that you must run with superuser privileges:

```shell
Output
[PM2] Init System found: systemd
[PM2] You have to run this command as root. Execute the following command:
sudo env PATH=$PATH:/usr/bin /usr/lib/node_modules/pm2/bin/pm2 startup systemd -u sammy --hp /home/sammy
```

Run the command that was generated (similar to the highlighted output above, but with your username instead of sammy) to set PM2 up to start on boot (use the command from your own output):

```
sudo env PATH=$PATH:/usr/bin /usr/lib/node_modules/pm2/bin/pm2 startup systemd -u sammy --hp /home/sammy
```

This will create a systemd unit which runs pm2 for your user on boot. This pm2 instance, in turn, runs hello.js. You can check the status of the systemd unit with systemctl:

```
systemctl status pm2-sammy
```

### Other PM2 Usage

PM2 provides many subcommands that allow you to manage or look up information about your applications. Note that running pm2 without any arguments will display a help page, including example usage, that covers PM2 usage in more detail than this section of the tutorial.

Stop an application with this command (specify the PM2 App name or id):

```
pm2 stop app_name_or_id
```

Restart an application with this command (specify the PM2 App name or id):

```
pm2 restart app_name_or_id
```

The list of applications currently managed by PM2 can also be looked up with the list subcommand:

```
pm2 list
```

More information about a specific application can be found by using the info subcommand (specify the PM2 App name or id):

```
pm2 info example
```

The PM2 process monitor can be pulled up with the monit subcommand. This displays the application status, CPU, and memory usage:

```
pm2 monit
```

Now your Node.js application is running, and managed by PM2.
