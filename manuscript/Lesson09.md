# Lesson 09 - Install a LAMP Server on an AMI

The following procedures help you install an Apache web server with PHP and MySQL support on your Amazon Linux instance (sometimes called a LAMP web server or LAMP stack). You can use this server to host a static website or deploy a dynamic PHP application that reads and writes information to a database.

> Note: If you are trying to set up a LAMP web server on an Ubuntu or Red Hat Enterprise Linux instance, this tutorial will not work for you.


## Overview

This tutorial assumes that you have already launched a new instance using the Amazon Linux AMI, with a public DNS name that is reachable from the internet. You must also have configured your security group to allow SSH (port 22), HTTP (port 80), and HTTPS (port 443) connections.


### Prerequisite: Complete the following tasks set up for Amazon EC2

1. Sign Up for AWS
2. Create an IAM User called Administrator with administrative access
3. Create a Key Pair
4. Create a Virtual Private Cloud (VPC)
5. Create a Security Group
6. Connect to your Instance

## Connect to you instance

1. Connect to your instance
2. From the command line of your instance, execute the following command: ```sudo yum update -y ```
3. Install the software packages and related dependencies.
```sudo yum install -y httpd24 php70 mysql56-server php70-mysqlnd  ```
4. Start the Apache web server.
```sudo service httpd start```
5. Use the chkconfig command to configure the Apache web server to start at each system boot.```sudo chkconfig httpd on ```.  The chkconfig command does not provide any confirmation message when you successfully use it to enable a service. You can verify that httpd is on by running the following command:    ```chkconfig --list httpd```
6. Add a security rule to allow inbound HTTP (port 80) connections to your instance:


   * Open the Amazon EC2 console at https://console.aws.amazon.com/ec2/. 
   * Choose Instances and select your instance. 
   * Under Security groups, choose view inbound rules. 
   * You should see the following list of rules in your default security group:
   ```cmd
      Security Groups associated with i-1234567890abcdef0
	Ports     Protocol     Source     launch-wizard-N
	22        tcp          0.0.0.0/0          
   ```
   *   Using the procedures in Adding Rules to a Security Group, add a new inbound security rule with the following values:
      * Type: HTTP 
      * Protocol: TCP 
      * Port Range: 80 
      * Source: Custom 

7. Test your web server. In a web browser, type the public DNS address (or the public IP address) of your instance. If there is no content in ```/var/www/html```, you should see the Apache test page.

    > You can get the public DNS for your instance using the Amazon EC2 console (check the Public DNScolumn; if this column is hidden, choose Show/Hide Columns (the gear-shaped icon) and choose Public DNS)

    >If you are unable to see the Apache test page, check that the security group you are using contains a rule to allow HTTP (port 80) traffic. For information about adding an HTTP rule to your security group, see Adding Rules to a Security Group. 

8. Complete the following steps so the ec2-user user (and any future members of the apache group) can add, delete, and edit files in the Apache document root. After that you will be ready to add content, such as a static website or a PHP application.

Apache httpd serves files that are kept in a directory called the Apache document root. The Amazon Linux Apache document root is ```/var/www/html```, which by default is owned by root. To allow the ```ec2-user``` account to manipulate files in this directory, you must modify the ownership and permissions of the directory.
* Add the ec2-user user to the apache group, to give the apache group ownership of the /var/www directory and assign write permissions to the group. Do this by running the following command: ```sudo usermod -a -G apache ec2-user```
* Log out and then log back in again to pick up the new group, and then verify your membership. To log out, type ```exit``` in the terminal. Once you re-connect, type ```groups``` and you should see ```ec2-user wheel apache``` as the response.
* Use the following command to change the group ownership of ```/var/www``` and its contents to the apache group: ```sudo chown -R ec2-user:apache /var/www```.
* To add group write permissions and to set the group ID on future subdirectories, change the directory permissions of /var/www and its subdirectories:
   ```sudo chmod 2775 /var/www```
   ```find /var/www -type d -exec sudo chmod 2775 {} \;```
* To add group write permissions, recursively change the file permissions of /var/www and its subdirectories:
    ```find /var/www -type f -exec sudo chmod 0664 {} \;```

### Test your LAMP Server

1. Type the following command to create a page which will print the php version to the browser.

```echo "<?php phpinfo(); ?>" >/var/www/html/phpinfo.php```

2. In a web browser, type the URL of the file that you just created. This URL is the public DNS address of your instance followed by a forward slash and the file name, for example: ```http://my.public.dns.amazonaws.com/phpinfo.php```

> If you do not see this page, verify that the /var/www/html/phpinfo.php file was created properly in the previous step. You can also verify that all of the required packages were installed with the following command:

```sudo yum list installed httpd24 php70 mysql56-server php70-mysqlnd```

> If any of the required packages are not listed in your output, install them using the sudo yum install package command.

3. Delete the ```phpinfo.php``` file. Although this can be useful information, it should not be broadcast to the internet for security reasons. Use this command: ```rm /var/www/html/phpinfo.php```


### Set up MySQL

1. ```sudo systemctl start mysqld```

```sudo systemctl status mysqld```

If MySQL has successfully started, the output should contain Active:
```active (running)``` and the final line should look something like:

```Dec 01 19:02:20 centos-512mb-sfo2-02 systemd[1]: Started MySQL Server.```

> During the installation process, a temporary password is generated for the MySQL root user. Locate it in the mysqld.log with this command:
```sudo grep 'temporary password' /var/log/mysqld.log```

2. Use this command to run the security script.
   ```sudo mysql_secure_installation```

This will prompt you for the default root password. As soon as you enter it, you will be required to change it.

3. Test MySQL  ```mysqladmin -u root -p version```

Your output should look like this:
You should see output similar to this:

```cmd
mysqladmin  Ver 8.42 Distrib 5.7.16, for Linux on x86_64
Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Server version          5.7.16
Protocol version        10
Connection              Localhost via UNIX socket
UNIX socket             /var/lib/mysql/mysql.sock
Uptime:                 2 min 17 sec

Threads: 1  Questions: 6  Slow queries: 0  Opens: 107  Flush tables: 1  Open tables: 100  Queries per second avg: 0.043
```

4. If you want the MySQL server to start at every boot, type the following command: ```sudo chkconfig mysqld on```

5. You should now have a fully functional LAMP web server. If you add content to the Apache document root at /var/www/html, you should be able to view that content at the public DNS address for your instance. You can also get your ip address by browsing to: ```https://whatismyipaddress.com/```.

### Install PHP MyAdmin

phpMyAdmin is a web-based database management tool that you can use to view and edit the MySQL databases on your EC2 instance. Follow the steps below to install and configure phpMyAdmin on your Amazon Linux instance.

1. Log in to your EC2 instance using SSH.

2. Install the required dependencies.
```sudo yum install php70-mbstring.x86_64 php70-zip.x86_64 -y```

3. Restart Apache. ```sudo service httpd restart```

4. Navigate to the Apache document root at /var/www/html.
```cd /var/www/html```

5. Run the following command:
```wget https://www.phpmyadmin.net/downloads/phpMyAdmin-latest-all-languages.tar.gz```

6. Extract and move the files
```cmd
tar -xvzf phpMyAdmin-latest-all-languages.tar.gz
mv phpMyAdmin-4.7.5-all-languages phpMyAdmin
```


7. Start MySQL service if it's not already running:
```sudo service mysqld start```

8. In a web browser, type the URL of your phpMyAdmin installation. This URL is the public DNS address (or the public IP address) of your instance followed by a forward slash and the name of your installation directory. For example: ```http://my.public.dns.amazonaws.com/phpMyAdmin```

9. You should see the phpMyAdmin login page

10. Log in to your phpMyAdmin installation with the root user name and the MySQL root password.

11. More to do ... your installation must still be configured before you put it into service. To configure phpMyAdmin, you can manually create a configuration file or use the setup console.



## What's going on?

You stated with an update on your instance. This process may take a few minutes, but it is important to make sure that you have the latest security updates and bug fixes. The -y option installs the updates without asking for confirmation. If you would like to examine the updates before installing, you can omit this option.

If you are not using Amazon Linux, you may also need to configure the firewall on your instance to allow these connections. For more information about how to configure the firewall, see the documentation for your specific distribution. 

## Questions

* What is SSH?
* What is a LAMP server?
* What is a firewall?
