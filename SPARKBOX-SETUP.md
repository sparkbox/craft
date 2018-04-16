# Sparkbox - Craft

A fork of Craft CMS - https://github.com/craftcms/craft

## Table of Contents

* [Install and Configure Craft](#install-and-configure-craft)
    * [Install and Configure Apache](#install-and-configure-apache)
    * [Installing MySQL](#installing-mysql)
    * [Install Sequel Pro](#install-sequel-pro)
    * [Sparkbox Craft Install](#sparkbox-craft-install)
    * [Create Craft Project Database](#create-craft-project-database)
    * [Run Craft](#run-craft)
* [Common Issues](#common-issues)
    * [Fields Not Saving](#fields-not-saving)
        
## Install and Configure Craft

### Install and Configure Apache

Follow the [Apache at Sparkbox](https://github.com/sparkbox/standard/tree/master/software/apache) guide in the Standard to install and configure Apache on your machine.

### Installing MySQL

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

### Sparkbox Craft Install

If you are creating the repo, you’ll need to run this command, which will grab all the composer files from our starter kit and setup the project. 

	$ composer create-project -s beta sparkbox/craft <your_project_name>

If you are cloning an existing already created project, but have not yet run composer install, you should be able to simply run that command inside the project directory.

	$ composer install
  
### Create Craft Project Database

Login to mysql as root

	$ mysql -u root -p

Create your database

	mysql create database db_name;

Check that your database exists

	mysql show databases;

If you want to create a specific user for that database (instead of using root), you can do that here as well

	mysql create user db_user;
	mysql grant select, insert, delete, update on db_name.* to 'db_user'@'localhost' identified by 'db_password';

Tell Craft how to access your database by updating the .env file in your project directory with the database name, user, and password.


### Run Craft

In your browser, go to `<your_project_name>.local/admin`. You should get a screen with a button to install Craft. Click that, create an account, and you should be good to go!

## Common Issues
### Fields Not Saving

If you run into a situation where you add a field inside the Craft CMS and it does not save (even though it appears to at first and there are no error messages) you may need to increase your PHP max-input-vars.

To fix this, add this line to your .htaccess file:

	php_value max_input_vars 5000

Make sure this change also gets made on the server for any versions of your app that are using a shared .htaccess file.
