## WEB STACK IMPLEMENTATION (LAMP STACK) IN AWS

### Introduction:

__The LAMP stack is a popular open-source web development setup with four key parts: Linux, Apache, MySQL, and PHP (or occasionally Perl or Python). This documentation explains how to install, set up, and use the LAMP stack.__

## Step 0: Prerequisites

__1.__ EC2 Instance of t2.micro type and Ubuntu 22.04 LTS (HVM) was launched in the eu-west-2 region using terraform.

![Lunch Instance1](./images/1-creating-ec2.png)
![Lunch Instance2](./images/3-creating-ec2.png)
![Lunch Instance3](./images/ec2-instance-summary-page.png)

__2.__ Created SSH key pair named __my-ec2-key-pair__ to access the instance on port 22.

__3.__ The security group was configured with the following inbound and outbound rules:

- Allow inbound SSH traffic on port 22 from a specified IP range to the security group.
- Allow inbound HTTP traffic on port 80 from any source IP to the security group.
- Allow all outbound traffic from the security group to any destination IP.

![Security Rules1](./images/inbound-rule.png)
![Security Rules2](./images/outbound-rule.png)

__4.__ A new custom VPC with CIDR block (10.0.0.0/16) and subnet CIDR block (10.0.0.0/20) was used for the networking configuration.

__5.__ The private SSH key (PEM file) was downloaded. The file was moved to the .ssh directory, permissions were changed for the PEM file to allow only the _user owner_ read access, and then it was used to connect to the instance by running a command.

```
chmod 400 my-ec2-key-pair.pem
```
```
ssh -i "my-ec2-key-pair.pem" ubuntu@35.178.185.57
```
Where __username=ubuntu__ and __public ip address=35.178.185.57__

![Connect to instance](./images/ssh-access.png)

## Step 1 - Install Apache and Update the Firewall

__1.__ __Update and upgrade list of packages in package manager__
```
sudo apt update
sudo apt upgrade -y
```
![Update Packages](./images/sudo-apt-update.png)

__2.__ __Run apache2 package installation__
```
sudo apt install apache2 -y
```
![Install Apache](./images/install-apache2.png)

__3.__ __Enable and verify that apache is running on as a service on the OS.__
```
sudo systemctl enable apache2
sudo systemctl status apache2
```
If it green and running, then apache2 is correctly installed
![Apache Status](./images/apache-enabled.png)

__4.__ __The server is running and can be accessed locally in the ubuntu shell by running the command below:__

```
curl http://localhost:80
OR
curl http://127.0.0.1:80
```
![Local URL](./images/apache-ubuntu-default-page-curl.png)

__5.__ __To check if the Apache HTTP server can handle requests from the internet, use the public IP address and enter it into a web browser's URL field for testing.__
```
http://35.178.185.57:80
```
![Apache Default Page](./images/apache-ubuntu-default-page-browser.png)

This shows that the web server has been successfully installed and can be reached despite the firewall restrictions.

__6.__ __Another method for obtaining the public IP address besides checking the AWS console.__
```
curl -s http://169.254.169.254/latest/meta-data/public-ipv4
```
![Public IP with curl](./images/public-ip-curl.png)

## Step 2 - Install MySQL

__1.__ __Install a relational database (RDB)__

MySQL was installed in this project, serving as a widely utilized relational database management system frequently employed in PHP environments.

```
sudo apt install mysql-server
```
![Install MySQL-1](./images/install-mysql-1.png)
![Install MySQL-2](./images/install-mysql-2.png)

When prompted, type the letter "y" on your keyboard. Press the "Enter" key to confirm the installation.

__2.__ __Enable and verify that mysql is running with the commands below__
```
sudo systemctl enable --now mysql
sudo systemctl status mysql
```
![MySQL Status](./images/check-mysql-status.png)

__3.__ __Log in to mysql console__
```
sudo mysql
```
This establishes a connection to the MySQL server using the administrative database user. 

Utilize the sudo command to grant your current user the capabilities of a root user.

__4.__ __Set up a password for the root user with mysql_native_password as the default authentication method.__

Here, the user's password was defined as "Admin123$"
```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'Admin123$';
```
![User Password](./images/mysql-login.png)
Exit the MySQL shell
```
exit
```
![Exit MYSQL shell-1](./images/exit-mysql-shell-1.png)

__5.__ __Run an Interactive script to secure MySQL__


The security script is included with MySQL by default. It's designed to eliminate certain vulnerable configurations and restrict access to the database system.

```
sudo mysql_secure_installation
```
![MYSQL secure installation-1](./images/mysql-secure-installation-1.png)
![MYSQL secure installation-2](./images/mysql-secure-installation-2.png)

Regardless of whether the VALIDATION PASSWORD PLUGIN is configured, the server will prompt for the selection and confirmation of a password for the MySQL root user.

__6.__ __After changing root user password, log in to MySQL console.__

A password prompt appeared in the command line interface following the execution of the command below.

```
sudo mysql -p
```
![](./images/access-mysql-with-password.png)

Exit MySQL shell
```
exit
```
![Exit MYSQL shell-2](./images/exit-mysql-shell-2.png)

## Step 3 - Install PHP

__1.__ __Install php__
Apache is installed to serve the content while MySQL is installed to store and manage data.
PHP is the component of the set up that processes code to display dynamic content to the end user.

The following were installed:
- php package
- php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases.
- libapache2-mod-php, to enable Apache to handle PHP files.

```
sudo apt install php libapache2-mod-php php-mysql
```
![Install PHP-1](./images/install-php-1.png)
![Install PHP-2](./images/install-php-2.png)

Confirm the PHP version
```
php -v
```
![Confirm php version](./images/confirm-php-installation.png)
At this point, the LAMP stack is completely installed and fully operational.

To test the setup with a PHP script, it's best to set up a proper Apache Virtual Host to hold the website files and folders. A Virtual Host allows for multiple websites to be located on a single machine without being noticed by the website users.

## Step 4 - Create a virtual host for the website using Apache

__1.__ __The default directory serving the apache default page is /var/www/html. Create your document directory next to the default one.__

Created the directory for projectlamp using "mkdir" command
```
sudo mkdir /var/www/projectlamp
```

__Assign the directory ownership with $USER environment variable which references the current system user.__
```
sudo chown -R $USER:$USER /var/www/projectlamp
```
![Projectlamp Root Directory](./images/projectlamp-directory.png)

__2.__ __Create and open a new configuration file in apache’s “sites-available” directory using vim.__
```
sudo vim /etc/apache2/sites-available/projectlamp.conf
```

Past in the bare-bones configuration below:
```
<VirtualHost *:80>
  ServerName projectlamp
  ServerAlias www.projectlamp
  ServerAdmin webmaster@localhost
  DocumentRoot /var/www/projectlamp
  ErrorLog ${APACHE_LOG_DIR}/error.log
  CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
![Virtual Host](./images/bare-bones-configuration.png)

__3.__ __Show the new file in sites-available__
```
sudo ls /etc/apache2/sites-available
```
```
Output:
000-default.conf default-ssl.conf projectlamp.conf
```
![Projectlamp config file](./images/ls-sites-available.png)

With the VirtualHost configuration, Apache will serve projectlamp using /var/www/projectlamp as its web root directory.

__4.__ __Enable the new virtual host__
```
sudo a2ensite projectlamp
```
![Enable virtual host](./images/new-virtual-host-enabled.png)

__5.__ __Disable apache’s default website.__

This is because Apache’s default configuration will overwrite the virtual host if not disabled. This is required if a custom domain is not being used.
```
sudo a2dissite 000-default
```
![Disable Apache default](./images/apache-default-website-disabled.png)

__6.__ __Ensure the configuration does not contain syntax error__

The command below was used:
```
sudo apache2ctl configtest
```
![Check syntax error](./images/check-syntax-error.png)

__7.__ __Reload apache for changes to take effect.__
```
sudo systemctl reload apache2
```
![Reload Apache](./images/reload-apache.png)

__8.__ __The new website is now active but the web root /var/www/projectlamp is still empty. Create an index.html file in this location so to test the virtual host work as expected.__
```
sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html
```
![Root dir content](./images/index-html-file.png)

__9.__ __Open the website on a browser using the public IP address.__
```
http://35.178.185.57:80
```
![URL public IP](./images/index-html-website.png)

This file can serve as a temporary landing page for the application until an index.php file is set up to replace it. Once this is done, the index.html file should be renamed or removed from the document root, as it will take precedence over the index.php file by default.

## Step 5 - Enable PHP on the website

With the default DirectoryIndex setting on Apache, the index.html file will always take precedence over the index.php file. This is useful for setting up a maintenance page in PHP applications by creating a temporary index.html file containing an informative message for visitors. The index.html then becomes the landing page for the application. Once maintenance is over, the index.html is renamed or removed from the document root, bringing back the regular application page. If the behavior needs to be changed, the **/etc/apache2/mods-enabled/dir.conf** file should be edited, and the order in which the index.php file is listed within the DirectoryIndex directive should be changed.

__1.__ __Open the dir.conf file with vim to change the behaviour__
```
sudo vim /etc/apache2/mods-enabled/dir.conf
```

```
<IfModule mod_dir.c>
  # Change this:
  # DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
  # To this:
  DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
```
![Change file list order](./images/index-php-config-behavior-change.png)

__2.__ __Reload Apache__

Apache is reloaded so the changes takes effect.
```
sudo systemctl reload apache2
```
![](./images/reload-apache-2.png)

__3.__ __Create a php test script to confirm that Apache is able to handle and process requests for PHP files.__

A new index.php file was created inside the custom web root folder.

```
vim /var/www/projectlamp/index.php
```
![PHP index file](./images/vim-new-php-file.png)

__Add the text below in the index.php file__
```
<?php
phpinfo();
```
![php text](./images/new-php-file.png)

__4.__ __Now refresh the page__

![PHP page](./images/server-info-php.png)

This page provides information about the server from the perspective of PHP. It is useful for debugging and ensuring that the settings are being applied correctly.

After reviewing the relevant information about the server on this page, it's best to delete the created file as it contains sensitive details about the PHP environment and the Ubuntu server. The file can always be recreated if the information is needed later.

```
sudo rm /var/www/projectlamp/index.php
```


__Conclusion:__

The LAMP stack offers a sturdy and adaptable framework for creating and launching web applications. By adhering to the instructions provided in this guide, we were able to establish, adjust, and manage a LAMP setup efficiently. This facilitated the development of potent and expandable web solutions.

