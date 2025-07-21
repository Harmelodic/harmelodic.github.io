# Installing an Apache HTTP Server

> Originally published: 23 September 2016

## Introduction

This guide is a guide to installing the Apache HTTP Server on a Debian based machine.

## Open a terminal

Open the applications menu/app drawer and find the one called *Terminal*.
OR use the shortcut: <kbd>Ctrl</kbd>+<kbd>Shift</kbd> <kbd>T</kbd>.

## Installation of Apache HTTP Server

Type in the following command:

```bash
sudo apt-get install apache2 apache2-doc apache2-utils
```

The above command does not contain PHP support to the installation, to install with PHP support, do:

```bash
sudo apt-get install apache2 apache2-doc apache2-utils php5 libapache2-mod-php5 php5-mcrypt
```

## Installation of Webapps

Simply put webapp files in the directory: `/var/www/html/`.
Then use the following command to restart the Apache HTTP Server, thus making your Webapps available:

```bash
sudo service apache2 restart
```
