---
layout: post
title: "Install Postgresql and PostGIS on AWS ubuntu 14.04 and Mac OSX"
description: "A complete guide to insall Postgresql and PostGIS on AWS ubuntu 14.04"
category: Database
tags: [postgresql, postgis, database, aws, ubuntu]
---
{% include JB/setup %}

## On Mac OSX

Because of [Homebrew](http://brew.sh/), installation is easier on Mac. Run these two commands to install both
postgresql and PostGIS

    $ brew install postgres
    $ brew install postgis

It is done, how nice!

### Start the service

type this command to start the postgresql service.

    $ pg_ctl -D /usr/local/var/postgres start

then check if the service is up and running.

    $ export PGDATA=/usr/local/var/postgres
    $ pg_ctl status`

### Make sure postgresql starts when mac starts up

If you don't want to run the above start command every time you start your mac, do the following to make sure
postgresql automatically starts.

1. make sure you have this directory `~/Library/LaunchAgents`. (create it if it does not exist)

2. run `ln -sfv /usr/local/opt/postgresql/*.plist ~/Library/LaunchAgents` to create a symbolic link for postgres plist
file

3. run `launchctl load ~/Library/LaunchAgents/homebrew.mxcl.postgresql.plist` to add the plist to launch control.

Postgresql should start automatically every time your mac starts.


## On AWS Ubuntu

Remote login to aws ubuntu instance, then run the following:

    $ sudo apt-get update
    $ sudo apt-get install -y postgresql postgresql-contrib


After installation, you can create database by running

`sudo -u postgres createdb DATABASE_NAME`

If you want to create a specific user for a database, run

    $ sudo -u postgres createuser -P USER_NAME
    $ sudo -u postgres createdb -O USER_NAME DATABSE_NAME

Then you can test database connection by running
`psql -U USER_NAME DATABSE_NAME`

### Install PostGIS

After postgresql is installed and connection tested, we can install postgis:

9.* is the version of your postgresql, and 2.* the version of PostGIS

    $ sudo apt-get install postgresql-9.*-postgis-2.*
    $ sudo apt-get install postgresql-server-dev-9.*

Then you can create PostGIS extension for your regular postgresql database to enable GIS functions.

    $ sudo -u postgres psql DATABASE_NAME

In psql session, do this to enable PostGIS and check if extension is created.

    db=# CREATE EXTENSION postgis;
    db=# select postgis_version();

Notes: if you use a created database user, make sure it has super role to create extension. If you are using GeoDjango,
migrate database using django manage.py command will create the extension automatically.


## Install PgAdmin III and connect to database

PgAdmin III is a s a comprehensive PostgreSQL database design and management system 

go to [http://www.pgadmin.org/download/macosx.php](http://www.pgadmin.org/download/macosx.php) to download and install
it.

After install, you can create a db connection to connect to your postgresql and PostGIS databases.

It is pretty straightforward to connect to a local database, just provide the localhost/port(default 5432) 
database username and password (if you have a password, or leave it empty).

If you want to connect to a remote database on AWS instance, you need to make some changes to config files

in /etc/postgresql/9.*/main/postgresql.conf add `listen_addresses='*'`

in /etc/postgresql/8.2/main/pg_hba.conf add `host all all 0.0.0.0/0 md5`

Because port 5432 (your postgresql port) may not be open by default security settings, you need to open it appropriately.
On amazon, you can add that rule in your security group of the instance in aws console.

After the changes, you should be able to connect to your remote database from your local machine.