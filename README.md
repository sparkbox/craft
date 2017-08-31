# Sparkbox - Craft

A fork of Craft CMS - https://github.com/craftcms/craft

## Table of Contents

   * [Sparkbox - Craft](#sparkbox---craft)
      * [Installing Xcode Libraries](#installing-xcode-libraries)
      * [Installing Homebrew](#installing-homebrew)
      * [Installing Apache](#installing-apache)
      * [Installing PHP](#installing-php)
      * [Setting up Apache](#setting-up-apache)
         * [Turning on PHP in Apache.](#turning-on-php-in-apache)
         * [Vhosts and the Hosts File](#vhosts-and-the-hosts-file)
      * [Installing MySQL](#installing-mysql)
         * [Install Sequel Pro](#install-sequel-pro)
      * [Installing Composer](#installing-composer)
      * [Project Setup](#project-setup)
         * [File Permissions](#file-permissions)
         * [Sparkbox Craft Install](#sparkbox-craft-install)
  
This guide will guide you to install:

* Homebrew

* Apache 2.4

* PHP 7.0

* MySQL 5.7 or greater

**Requirements (Getting your system ready).**

This document assumes that the reader has a MacBook of some kind, with the latest Operating System (MacOS Sierra 10.12.6 at the time of writing). The reader also must be able to issue commands in the terminal, must have some basic understanding of the underlying operating system and file system. 

This guide is exactly the steps that need to be followed to install Homebrew, Apache, and PHP. 

[https://getgrav.org/blog/macos-sierra-apache-multiple-php-versions](https://getgrav.org/blog/macos-sierra-apache-multiple-php-versions) 

## Installing Xcode Libraries

 	$ xcode-select --install

## Installing Homebrew

$ /usr/bin/ruby -e "$(curl -fsSL [https://raw.githubusercontent.com/Homebrew/install/master/install](https://raw.githubusercontent.com/Homebrew/install/master/install))"

	$ brew --version
	
	$ brew doctor
	
	$ brew tap homebrew/dupes
	
	$ brew tap homebrew/versions
	
	$ brew tap homebrew/php
	
	$ brew tap homebrew/apache
	
	$ brew update

## Installing Apache

	**$ which apachectl**
	
	$ sudo apachectl stop
	
	$ sudo launchctl unload -w /System/Library/LaunchDaemons/org.apache.httpd.plist 2>/dev/null
	
	$ brew install httpd24 --with-privileged-ports --with-http2
	
	$ sudo cp -v /usr/local/Cellar/httpd24/**2.4.27**/homebrew.mxcl.httpd24.plist /Library/LaunchDaemons
	
	$ sudo chown -v root:wheel /Library/LaunchDaemons/homebrew.mxcl.httpd24.plist
	
	$ sudo chmod -v 644 /Library/LaunchDaemons/homebrew.mxcl.httpd24.plist
	
	$ sudo launchctl load /Library/LaunchDaemons/homebrew.mxcl.httpd24.plist
	
	$ sudo apachectl -k restart

**Useful commands to know:**

	$ sudo apachectl start
	
	$ sudo apachectl stop
	
	$ sudo apachectl -k restart

**Important file locations:**

	/usr/local/etc/apache2/2.4/httpd.conf

## Installing PHP

	$ brew install php56 --with-httpd24
	
	$ brew unlink php56
	
	$ brew install php70 --with-httpd24

**Important**** file locations: **
	
	/usr/local/etc/php/5.6/php.ini
	
	/usr/local/etc/php/7.0/php.ini

## Setting up Apache

Choose a "web root". For example, a good place to put web files would be in your home directory under `www`: 

	~/www/

Create a default VHost

In your web root.

	$ echo "Hello there" > index.html
	
	$ echo "<?php phpinfo();" > info.php

Edit your Apache Configuration File, DocumentRoot to the following. (Replace the existing document root with your document root):
	
	DocumentRoot "/Users/<your_username>/www"
	
	<Directory "/Users/<your_username>/www">
	
	    Options Indexes FollowSymLinks
	
	    AllowOverride All
	
	    Require all granted
	
	</Directory>

	DocumentRoot "/var/www"

Add

	Include /usr/local/etc/apache2/2.4/vhosts/*
	
	User/Group
	
	User _www
	
	Group _www

### Turning on PHP in Apache.

Open the Apache Config file ( /usr/local/etc/apache2/2.4/httpd.conf )

**Edit the following lines:**
	
	LoadModule php5_module        /usr/local/Cellar/php56/5.6.26_3/libexec/apache2/libphp5.so
	
	LoadModule php7_module        /usr/local/Cellar/php70/7.0.11_5/libexec/apache2/libphp7.**so**

**To the following: **
	
	# LoadModule php5_module    /usr/local/opt/php56/libexec/apache2/libphp5.so
	
	LoadModule php7_module    /usr/local/opt/php70/libexec/apache2/libphp7.so
	
**Edit the following lines:**

	<IfModule dir_module>
	
	    DirectoryIndex index.html
	
	</IfModule>

**To the following:**

	<IfModule dir_module>
	
	    DirectoryIndex index.php index.html
	
	</IfModule>
	
	<FilesMatch \.php$>
	
	    SetHandler application/x-httpd-php
	
	</FilesMatch>

### Vhosts and the Hosts File

Using Apache 2.4  you should be able to add vhosts using the vhost directory.

In your local apache `/usr/local/etc/apache2/2.4/ directory run the following commands.

	$ mkdir vhosts
	
	$ cd vhosts
	
	$ touch <your_project_name>.conf

Open the file <your_project_name>.conf

Copy and paste the following vhost configuration inside that file:

	<VirtualHost 127.0.0.1:80>
	
	    ServerAdmin **youremail@heysparkbox.com**
	
	    DocumentRoot "**<your_path_to_files>/<your_project_name>/web**"
	
	    ServerName **<your_project_name>.dev**
	
	    <Directory "**<your_path_to_files>/<your_project_name>/web**">
	
	       Options Indexes MultiViews FollowSymLinks
	
	       AllowOverride All
	
	       Require all granted
	
	       DirectoryIndex index.php
	
	   </Directory>
	
	</VirtualHost>

You’ll need to swap out the bolded text with your own settings.

Save this file, and restart apache.

Host file, open with `sudo` to make changes to /etc/hosts

Add the following to the end of the file.

	127.0.0.1 <your_project_name>.dev

## Installing MySQL

We will not be using homebrew for mysql installations.

Install the MySQL Binary: [https://dev.mysql.com/downloads/mysql/](https://dev.mysql.com/downloads/mysql/) 


Download and follow setup instructions.

**For MySQL 5.7 +**

**After installing, you’ll need to do the following:**

	$ touch /etc/my.cnf

Add:


	[mysqld]
	sql_mode=STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION

[https://craftcookbook.net/recipes/making-craft-play-nicely-with-mysql-5-7-5](https://craftcookbook.net/recipes/making-craft-play-nicely-with-mysql-5-7-5) 

### Install Sequel Pro

Using the link:

[https://sequelpro.com/download#auto-start](https://sequelpro.com/download#auto-start) 

Download and install the binary for Sequel Pro. This (free) application will be able to create databases, users, and export/import data from external databases. 

## Installing Composer

We’ll be installing composer globally. 

	$ cd ~
	
	$ wget https://raw.githubusercontent.com/composer/getcomposer.org/1b137f8bf6db3e79a38a5bc45324414a6b1f9df2/web/installer -O - -q | php -- --quiet
	
	$ mv composer.phar /usr/local/bin/composer

## Project Setup

### File Permissions

	$ chgrp -R* <group_name>* **<path_to_project>**

Then run:

	$ chmod -R 774 **/path_to_project/<your_project_name>**
	
	$ chmod -R g+rwx,o+rwx **/path_to_project/<your_project_name>**

### Sparkbox Craft Install

If you are creating the repo, you’ll need to run this command, which will grab all the composer files from our starter kit and setup the project. 

	$ composer create-project -s beta sparkbox/craft <your_project_name>

If you are cloning an existing already created project, but have not yet run composer install, you should be able to simply run that command inside the project directory.

	$ composer install


