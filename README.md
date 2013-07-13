doc
===
Installation & Configuration
Table of Contents

    Getting Started

    1.1. FonB Solution Architecture

    System Requirements

    2.1. OS Requirements

    2.2. Asterisk Version Support

    2.3. Browser Requirement

    Package Dependencies

    3.1. PHP Installation

    3.2. PHP-MySQL Installation

    3.3. PHP-CGI Installation

    3.4. MySQL Server Installation

    3.5. MySQL Client Installation

    3.6. PHP-XML Installation

    Downloading FonB

    Installation & Configuration

    5.1. Decompress TAR Files

    5.2. Create phoneb.cfg File

    5.3. Create users.cfg file

    5.4. Activate License

    5.5. Permit Subnet in manager.conf

    5.6. Create FonB MySQL Database

    5.7. Configure IonCube Encoder

    5.8. Modify Asterisk MySQL CDR DB For Highrise Entries

1. Getting Started

FonB is web based software addon to provide additional features to your Asterisk Server. These features include Phonebook for Asterisk, Highrise CRM Integration, FonB XML App for IP Phones, Call History XML App for IP Phones, Active Call Panel, Call Switching, Number Lookup, Number Rewrites etc. This document provides instructions to download, install and configure FonB from scratch. It is assumed that you have a fully functional Asterisk Server running in environment where FonB is to be installed. For integration with Highrise, it's assumed that you have a working Highrise CRM account to be integrated with Aptus FonB. Please be noted that this document is expected to be modified with time based on user input we are going to receive. Please feel free to report any issues in support forum.
1.1. FonB Solution Architecture

FonB connects with Asterisk, Google, MySQL and Highrise (based on license type). With Asterisk, FonB needs to communicate with Asterisk AMI (manager.conf), Asterisk Dialplan (extensions.conf and extensions_custom.conf), Asterisk Call Recording Files (xxxxxx) and Asterisk CDR Database.

FonB communicates with MySQL for two reasons, first to handle its own records in a database and secondly, to deal with Asterisk CDR records.

User is supposed to inform FonB about these files in 3 configuration files, i.e. phoneb.cfg, users.cfg and php.ini (within FonB Installation directory), as explained in this document, section 5.

2. System Requirements

FonB can be installed on any standard x86/x86_64 based hardware system. Operating System and Browser specific requirements are as under:
2.1. OS Requirements

2.2. Asterisk Version Support

2.3. Browser Requirement

Mozilla Firefox 6, Mozilla Firefox 5, Google Chrome 14, Safari 5, Opera 11, Internet Explorer 9, Internet Explorer 8

3. Package Dependencies

FonB requires these packages to be installed and work properly:

    PHP
    PHP-MySQL
    PHP-CGI
    MySQLServer
    MySQLClient
    PHP-XML

If you are installing FonB on servers running Asterisk forks like FreePBX, Elystix or PIAF, you most probably will have PHP, PHP-MySQL, MySQLServer & MySQLClient already installed.
3.1. PHP Installation

For CentOS based systems yum install php

For Debian based systems apt-get install php
3.2. PHP-MySQL Installation

For CentOS based systems yum install php-mysql

For Debian based systems apt-get install php-mysql
3.3. PHP-CGI Installation

For CentOS based systems yum install php-cgi

For Debian based systems apt-get install php-cgi
3.3. MySQL Server Installation

For CentOS based systems yum install mysql-server

For Debian based systems apt-get install mysql-server

You can install specific version however it should comply the requirements as described in section 3.
3.4. MySQL Client Installation.

For CentOS based systems yum install mysql-client

For Debian based systems apt-get install mysql-client

You can install specific version however it should comply the requirements as described in section 3.
3.5. PHP-XML Installation.

For CentOS based systems yum install php-xml

For Debian based systems apt-get install php-xml

4. Downloading FonB Executables

FonB is a commercial software available for 30 days free trial in a compressed .tar format here. After 30 days, FonB will stop working and you will need to buy a license using a credit card. Once the license is purchased, it will be emailed to you on the registered email, along with the instructions to activate it (also explained here in Section 6).

Make sure your OS and Asterisk version is specified in Section 2. If it's not, just wait while we are working to release it on many other Linux distros and Asterisk forks.

5. Installation & Configuration

The download link will provide you phoneb.tar file. Download it and place it in the directory you want to install FonB in. In our case, we will be installing PhoneB in /usr/local directory, so place the downloaded file here. From here, follow the given steps:

5.1. Decompress the tar files:

tar xvzf phoneb.tgz
5.2. Setting phoneb.cfg file

FonB executable is found in /phoneb/bin/ directory. In order to run FonB, you need to create two files, i.e. phoneb.cfg and users.cfg. These files can be created in any of these directories: /etc/phoneb/phoneb.cfg /usr/local/phoneb/conf/phoneb.cfg /usr/local/etc/phoneb/phoneb.cfg ./phoneb.cfg ../conf/phoneb.cfg /usr/local/phoneb/phoneb.cfg ../phoneb.cfg Please note that phoneb.cfg and users.cfg should be in a same directory. For the sake of example, we will be creating these two files in /etc/phoneb/. you need to create this directory by mkdir /etc/phoneb/
5.2.1. Sample phoneb.cfg

Here is the sample phoneb.cfg file:

[PhoneB]
BaseDir= /usr/local/phoneb             ;Path for FonB Installation
UsersConfFile= /etc/phoneb/users.cfg   ;Path for users.cfg and phoneb.cfg file

[WebServer]
ListenPort= 80                         ;Web server port
Debug= 0

[AMI]
ManagerHost= localhost                 ;Your Asterisk Hostname/IP
ManagerPort= 5038                      ;AMI Port
ManagerUsername= amiuser               ;AMI Username set in manager.conf
ManagerPassword= amipassword           ;AMI Password set in manager.conf
Debug= 0

[WebSocket]
Debug= 1
ListenPort= 80
EnableAmiUpdates= 0
EnableDetailedUpdates= 0
EnableSummaryUpdates= 1
EnableHangupUpdates= 1
EnableErrorUpdates= 1
EnableClientActionUpdates= 1

[MysqlCdr]
Username= freepbxuser                  ;Your Asterisk/FreePBX MySQL CDR username as found in cdr_mysql.conf
Password= freepbxpass                  ;Your Asterisk/FreePBX MySQL CDR password as found in cdr_mysql.conf
Database= asteriskcdrdb                ;Your Asterisk/FreePBX MySQL CDR database as found in cdr_mysql.conf
Hostname= localhost                    ;Your Database Server Address
Table= cdr                             ;Your Asterisk/FreePBX MySQL CDR Table name

[MysqlFonB]                            ;This section defines the database for FonB. You will need to create
                                       ;it by yourself
Username= fonbuser                     ;username for FonB Database
Password= fonbpass                     ;password for FonB Database
Database= phonebook                    ;FonB database name
Hostname= localhost                    ;Database Server Hostname/IP for FonB Database

5.3. Create users.cfg File

The configuration file users.cfg enables Asterisk users to use Aptus FonB. All the user extensions specified here must be the part of Asterisk extensions.conf file.
5.3.1. Sample users.cfg

Here is the sample users.cfg file:

[3000]                                 
Extension=3000                         ;Extension
Terminal=SIP/3000                      ;Extension type as defined in Extensions.conf
Context=from-internal                  ;Extension context
Name=max                               ;Name of the user
Mobile=0124567890                      ;Mobile Number
Password=3333                          ;Password for Weblogin
Language=en                            ;Language
DID=4547733                            ;Internal DID 
Department=Development                 ;Department Name
Spy=all                                ;Listen to all Other Calls
Company=aptus                          ;Company Name

[3001]
Extension=3001
Terminal=SIP/3001
Context=from-internal
Name=Garcia
Mobile=0134982392
Password=abc123
spy=Development,HR                     ;Listen to specific departments

5.4. Activate License

You need to run /usr/local/phoneb/bin/phoneb executable once phoneb.cfg and users.cfg are configured properly. If everything is good, you will be prompted for Activation License. If you have purchased FonB, you must enter the license key you have been provided on your registered email. In case of free 30 days demo, you just need to press enter. You must have an Internet Connection at this stage.

[root@localhost phoneb]# /usr/local/PhoneB/bin/phoneb 
2013/07/12 10:13:39 phoneb.cfg located @  /etc/phoneb/phoneb.cfg
2013/07/12 10:13:39 PhoneB BaseDir: /usr/local/PhoneB
2013/07/12 10:13:39 users.cfg located @  /etc/phoneb/users.cfg
2013/07/12 10:13:39 License not found or not complete
Please entre your activation key OR entre nothing to request a 30 day demo license. (Internet is required)
Activation Key:  
Demo_License_FonB_V1 license was retrieved from the activation server. This license will be deployed on your next FonB restart.
[root@localhost phoneb]# 

5.5. Permit Subnet in Manager.conf

In your /etc/asterisk/manager.conf search for deny/permit setting for different IPs/Subnets and change it to permit your subnet for connectivity. This step is mandatory as of now, even if you are running Asterisk and FonB on same machine.

deny=0.0.0.0/0.0.0.0
permit=10.0.8.0/255.255.255.255
permit=127.0.0.1/255.255.255.0

5.6. Create FonB MySQL Database

FonB uses MySQL Database to store all of its information. At this stage, you need to create a database to be used by FonB.

mysql -u root -p
Enter password: yourmysqlpassword
mysql> CREATE DATABASE phonebook;
mysql> CREATE USER 'username'@'localhost' IDENTIFIED BY 'userpassword';
mysql> GRANT ALL PRIVILEGES ON *.* TO 'username'@'localhost' WITH GRANT OPTION;

5.7. Configure Ioncube Encoder

In order to display the PHP pages, you need to configure Ioncure Encoder bundled with FonB. Prior to that, you need to confirm if your system is 32 bit (x86) or 64 bit (x86_64). Use uname -m to check this.

For 64bit system it will be like:

max@max-ThinkPad-T430:/$ uname -m
x86_64

For 32bit system, it will be like:

[root@localhost ~]# uname -m
i686

Secondly, check out the PHP version you are running, using php -v:

[root@localhost ~]# php -v
PHP 5.3.3 (cli) (built: May  7 2012 19:58:17) 
Copyright (c) 1997-2010 The PHP Group
Zend Engine v2.3.0, Copyright (c) 1998-2010 Zend Technologies
    with Zend Guard Loader v3.3, Copyright (c) 1998-2010, by Zend Technologies

You need to commit a couple of changes now based on your System Architecture (32 or 64 bit) and the PHP version you are running (5.3.3 in this case). Edit /usr/local/phoneb/php/php.ini:

date.timezone = "Asia/Dubai"                ;Your time zone, important to correctly display Call History
magic_quotes_gpc = Off
magic_quotes_runtime = Off
max_execution_time = 0
max_input_time = 180
register_argc_argv = On
file_uploads = On
upload_tmp_dir = /tmp
upload_max_filesize = 55M
post_max_size = 55M
html_errors = Off
include_path = ".:/etc/inc:./classes:./include"
log_errors = On
error_log = "/tmp/php4.log"
display_startup_errors = Off
auto_detect_line_endings = On
; extension=mysql.so
session.save_path=/tmp/sessions
zend_extension = ./ioncube32/ioncube_loader_lin_5.3.so  ;this needs to be changed as per your system

The parameter zend_extension = ./ioncube32/ioncube_loader_lin_5.3.so should be changed as per your system architecture and PHP version. For 32 bit systems running PHP 5.3, it will remain as it is. For 64 bit systems running PHP 5.3 it will become: zend_extension = ./ioncube64/ioncube_loader_lin_5.3.so

For 64 bit system running PHP 5.1 it will be:

zend_extension = ./ioncube64/ioncube_loader_lin_5.1.so

And so on for any other version of PHP.

5.8. Modify Asterisk MySQL CDR DB For Highrise Entries

In order to save Highrise related data to your Asterisk CDR, you will be required to modify your MySQL CDR Database.

mysql -u root -p
Enter password: yourmysqlpassword

mysql> ALTER TABLE cdr ADD FonBCallUniqueID VARCHAR(80) NOT NULL;
Query OK, 3040 rows affected (0.07 sec)
Records: 3040  Duplicates: 0  Warnings: 0

mysql> ALTER TABLE cdr ADD FonBCallNotes VARCHAR(80) NOT NULL;
Query OK, 3040 rows affected (0.08 sec)
Records: 3040  Duplicates: 0  Warnings: 0

mysql> ALTER TABLE cdr ADD FonBHighriseNoteID VARCHAR(80) NOT NULL;
Query OK, 3040 rows affected (0.08 sec)
Records: 3040  Duplicates: 0  Warnings: 0
