# Setting up a large Minecraft server

> Originally published: 23 September 2016

## Introduction

This is a guide on how to set up a Spigot Server on Linux using Multicraft, BungeeCord and tmux.

We're going to be using a Ubuntu 14.04 Server on AWS along with my domain name harmelodic.com so we can fiddle with
domain names too.

When this will be put into production, I'll be using a Debian 8 installation but as Ubuntu 14.04 is based on Debian, I
don't foresee any major changes to the setup process (with the possible exception of small configuration changes)

There will be a few occasions during this blog where I will mention configuring or doing something on your own. This is
usually because I'll be doing something that involves private information or it will be so long-winded and tedious that
it's not worth mentioning.  
If you aren't sure of what to do in these scenarios, I'd highly recommend reading the related documentation.

## AWS Instance Setup

- Ubuntu 14.04 Server (64-bit)
- 8GB RAM

## Preparation!

Add a user for minecraft stuff called *minecraft* and add the user to the *sudo* group so that it can perform `sudo`
commands:

```bash
sudo adduser minecraft
sudo adduser minecraft sudo
su - minecraft
```

Setup Java 8:

```bash
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java8-installer
java -version
```

Install Git and tmux:

```bash
sudo apt-get install git-all tmux
```

Download the archives we need into their own folders:

Download the BuildTools.jar and run it to get the most recent version of Spigot:

```bash
mkdir spigot
cd spigot
wget -O BuildTools.jar "https://hub.spigotmc.org/jenkins/job/BuildTools/lastSuccessfulBuild/artifact/target/BuildTools.jar"
sudo java -jar BuildTools.jar
cd ..
```

Download the BungeeCord archive:

```bash
mkdir bungee
cd bungee
wget -O BungeeCord.jar "http://ci.md-5.net/job/BungeeCord/lastSuccessfulBuild/artifact/bootstrap/target/BungeeCord.jar"
cd ..
```

## Installation of Multicraft

First thing to setup is Multicraft.  
Change directory to `/home/minecraft/`:

```bash
wget -O multicraft.tar.gz "http://www.multicraft.org/download/linux64"
sudo tar xvzf multicraft.tar.gz
cd multicraft
sudo ./setup.sh
```

Follow the instructions and install multicraft.

You'll have noticed in the install instructions that we need to run install.php file.  
This requires PHP and the PHP sqlite/mysql extension(s) (both of which don't come auto-installed on AWS Ubuntu Servers.
This also assumes you're using a sqlite database).  
Install PHP:

```bash
sudo apt-get install php5
sudo apt-get install php5-sqlite
sudo apt-get install php5-mysql
sudo apt-get install php5-mysqlnd
sudo service apache2 restart
```

I'm going to use MySQL as a database as it's a little easier to configure; As such I need to install MySQL server, login
to the MySQL CLI and create the 2 databases needed:

```bash
sudo apt-get install mysql-server
sudo service mysql start
mysql -u root -p
```

then in the MySQL shell:

```sql
CREATE
DATABASE multicraft_panel;
CREATE
DATABASE multicraft_daemon;
```

Ensure the databases are showing up inside MySQL Server with:

```sql
SHOW
DATABASES;
```

Then exit out of the MySQL CLI.

Open a web browser and go to `http://<your-server-ip>/multicraft/install.php` (You may need to edit your AWS Security
Group to allow HTTP connections)

Follow the installation process, you may need to do a little fiddling with the webserver to make sure everything on the
install list succeeds!

On the last page, you'll be shown the Daemon Configuration and at the bottom your Daemon. If you don't see your Daemon,
perform:

```bash
sudo /home/minecraft/multicraft/bin/multicraft start
```

I found a couple of issues with this.  
Firstly, it failed to start the TCP server, as it was already running. I had to find the PID number of the multicraft
service and then kill the service:

```bash
sudo netstat -tulpn | grep :25465
kill -9 <pid_number>
```

I then had an issue where it just didn't appear in the Daemon list, but seemed to start. After performing the start up
command with Debug mode on:

```bash
sudo /home/minecraft/multicraft/bin/multicraft -n start
```

I found it couldn't connect to MySQL database as the database connection details it had in
`/home/minecraft/multicraft/multicraft.conf` were empty.

Once the daemon appears, you can go to `http://<your-server-ip>/multicraft/index.php`, perform:

```bash
sudo rm /var/www/html/multicraft/install.php
```

And you're done!

## Installation of servers/Spigot

From the Multicraft menu, you can just create your servers! But before you can actually START them, you'll need to use
the most recent Spigot jar.

Thankfully in Multicraft if you go to `Settings > Update Minecraft` you can download, install and update versions of the
Minecraft Server (including Spigot).

At the time of writing this, Minecraft and Spigot version 1.9.2 is the most recent Minecraft version. Unfortunately,
Multicraft would only update Spigot to version 1.9.0 so I'm going to have to add a custom Spigot jar.

In our preparation we've already got the most recent version of Spigot generated (by running BuildTools.jar). So all we
need to do is:

```bash
cp /home/minecraft/spigot/spigot-1.9.2.jar /home/minecraft/multicraft/jar/.
```

Now that the most recent version of spigot is multicraft, let's go back to the Multicraft panel.

Create a server and only enter the `Name` field. All other fields will be set to default and can later be configured.

Once the server has been created, Go to `Servers` and click on the server we've just created.

Go down to `JAR File`, select `Default` in the upper drop-down menu and type `spigot-1.9.2.jar`.  
Click <kbd>Save</kbd>  
Then, click <kbd>Accept EULA</kbd>  
Click <kbd>Save</kbd> again.

Now start your server! You should be able to connect by going into Minecraft and using `<your-server-ip>`.

As we're going to be using BungeeCord as a front end proxy, we'll need to shutdown each server and change the port
number for each server to `25566`, then `25567`, then `25568` and so on for each server we've setup.

## Installation of BungeeCord

Ah...BungeeCord! A lot of people hate BungeeCord and complain it's too complicated.

I suspect these people don't really understand what BungeeCord is. (FYI it's just a proxy server designed to handle
multiple Minecraft Servers)

As per our preparation we already have BungeeCord.jar in `/home/minecraft/multicraft/bungee/`. What we need to do now is
generate the configuration files. To do this we need to run BungeeCord:

```bash
java -jar BungeeCord.jar
```

Once you see:

> 18:48:23 [INFO] Listening on /0.0.0.0:25577  
> \>

Hit <kbd>Ctrl</kbd>+<kbd>C</kbd> to end BungeeCord running.  
If you run `ls` now, you'll see we have a load of configuration files!

Open `config.yml` in your favourite terminal editor and configure it how you want it to run! However I'd *highly*
recommended setting the following things:

Under `servers:`, define all your servers that you're using and name them how you want. By default you'll have one
server there already called `lobby:`.  
You'll want to define the `address:` for each server to `localhost:` with `25566`, then `25567`, then `25568` and so on.
The default minecraft server port is `25565` but we're going to use that for BungeeCord itself. This will mean that when
we do `/server <server_name>` in game, we can change/jump to that server! Wooo!  
Speaking of which, under `listeners:`, define `query_port:` to `25565` and then define `host:` to `0.0.0.0:25565`.

Once you've defined everything, open a *tmux* session called *bungee*, run BungeeCord then detach from the session:

```bash
tmux new -s bungee
java -jar BungeeCord.jar
```

<kbd>Ctrl</kbd>+<kbd>B</kbd> <kbd>D</kbd>

BungeeCord is now set up and running! We're done!

## Installing Plugins and Worlds

If we want to install any new *plugins* to our server, we can either:

- Dump and configure them in `/home/minecraft/multicraft/servers/server<#>/plugins`.  
  or
- FTP to our server and install them through there. (FTP Access can be found out from
  `Servers > <server_name> > Files > FTP File Access`).

If we want to install any new *worlds* to our server, we can either:

- Dump and configure them in `/home/minecraft/multicraft/servers/server<#>/plugins`.  
  or
- FTP to our server and install them through there. (FTP Access can be found out from
  `Servers > <server_name> > Files > FTP File Access`).

For the latter (FTP) option, we'd need to install a FTP client on your server and configure it with Multicraft.

Remember you'll if you're installing a new plugin on a server you'll need to restart that server for the plugin to be
registered.

## Domain Name Registration

I'm going to be using AWS's *Route 53* to configure DNS Registration but most DNS providers will work in a similar way.

Within a domain's hosted zone, simply create a new *Record Set* that is of type *A*.

So for me, I'm entering my `harmelodic.com` hosted zone and creating an *A* type *Record Set*.

Then name it your chosen sub-domain (I'm going to use `mc.harmelodic.com`) and define the value of the *Record Set* to
`<your-server-ip>`.

Create/Confirm it and you're done!

## End-to-end pattern

Assuming you've done all the above, you should now have a server that maps as follows:

1. Requests going to `mc.DOMAIN.TLD` are directed to your dedicated server.
2. BungeeCord (running in a tmux session) listens at port `25565` for your normal Minecraft requests.
3. BungeeCord redirects requests to a Multicraft managed Minecraft Server (defaulting to a *lobby* styled server).
4. Each server has it's own plugins and worlds.

Congratulations!
