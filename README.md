# How to Create a Sudo User on Ubuntu 
```
cat ~/.ssh/id_rsa.pub | ssh root@your.ip.addr.ss 'cat - >> ~/.ssh/authorized_keys'
```
```
ssh root@server_ip_address

```
```
adduser username
```
```
Set password prompts:
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully

User information prompts:
Changing the user information for username
Enter the new value, or press ENTER for the default
    Full Name []:
    Room Number []:
    Work Phone []:
    Home Phone []:
    Other []:
Is the information correct? [Y/n]
```
## Grant a User Sudo Privileges

**Never edit this file with a normal text editor! Always use the visudo command instead!**
```
visudo
or 
sudo visudo
```
Search for the line that looks like this:
```
root    ALL=(ALL:ALL) ALL
```
Below this line, copy the format you see here, changing only the word "root" to reference the new user that you would like to give sudo privileges to:

```
root    ALL=(ALL:ALL) ALL
newuser ALL=(ALL:ALL) ALL
```
```
usermod -aG sudo username
```
Test sudo access on new user account
```
su - username

```
check root
```
sudo ls -la /root
```
## Add a existing user to www-data group

```
sudo usermod -a -G www-data  username
id username
groups username
```
# How To Set Up a Firewall with UFW on Ubuntu 

## Step 1:  — Using IPv6 with UFW (Optional)
```
sudo nano /etc/default/ufw
```
Then make sure the value of IPV6 is yes. It should look like this:
```
IPV6=yes
```
## Step 2 — Setting Up Default Policies

Let's set your UFW rules back to the defaults so we can be sure that you'll be able to follow along with this tutorial. To set the defaults used by UFW, use these commands:
```
sudo ufw default deny incoming
sudo ufw default allow outgoing
```
## Step 3: — Allowing SSH Connections
```
sudo ufw allow ssh
or 
sudo ufw allow 22
or 
# make sure you remember port number
sudo ufw allow 2222
```
## Step 4: — Enabling UFW

```
sudo ufw enable
```
## Step 5: — Allowing Other Connections

```
sudo ufw allow "your port number"
or 
sudo ufw allow 6000:6007/tcp
# Specific IP Addresses
sudo ufw allow from 15.15.15.51

```
check status
```
sudo ufw status verbose
```
# How To Set Up Apache Virtual Hosts on Ubuntu 

```
sudo apt-get update
sudo apt-get install apache2
```
## Step 1: — Create the Directory Structure
```
sudo mkdir -p /var/www/example.com/public_html
sudo mkdir -p /var/www/test.com/public_html
```
## Step 2: — Grant Permissions
```
sudo chown -R $USER:$USER /var/www/example.com/public_html
sudo chown -R $USER:$USER /var/www/test.com/public_html
```
```
sudo chmod -R 755 /var/www
```
## Step 3: — Create Demo Pages for Each Virtual Host
```
nano /var/www/example.com/public_html/index.html

```
Add this:
```
<html>
  <head>
    <title>Welcome to Example.com!</title>
  </head>
  <body>
    <h1>Success!  The example.com virtual host is working!</h1>
  </body>
</html>
```
## Step 4: — Create New Virtual Host Files
```
sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/example.com.conf
```
Open the new file in your editor with root privileges:
```
sudo nano /etc/apache2/sites-available/example.com.conf
```
```
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
First, we need to change the `ServerAdmin` directive to an email that the site administrator can receive emails through.
```
ServerAdmin admin@example.com
```
After this, we need to add two directives. The first, called ServerName, establishes the base domain that should match for this virtual host definition. This will most likely be your domain. The second, called ServerAlias, defines further names that should match as if they were the base name. This is useful for matching hosts you defined, like www:
```
ServerName example.com
ServerAlias www.example.com
DocumentRoot /var/www/example.com/public_html
```
In total, our virtualhost file should look like this:
```
<VirtualHost *:80>
    ServerAdmin admin@example.com
    ServerName example.com
    ServerAlias www.example.com
    DocumentRoot /var/www/example.com/public_html
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
## Step 5: — Enable the New Virtual Host Files
```
sudo a2ensite example.com.conf
sudo a2ensite test.com.conf
```
```
sudo service apache2 restart
```
Test your results
```
http://example.com
```

# How To Intall LAMP in DigitalOcean

## Step 1: Install Apache

```
sudo apt-get update
sudo apt-get install apache2
```
```
http://your_server_IP_address
```
## Step 2: Install MySQL

```
sudo apt-get install mysql-server php5-mysql
```

```
sudo mysql_install_db
sudo mysql_secure_installation
```

## Step 3: Install PHP

```
sudo apt-get install php5 libapache2-mod-php5 php5-mcrypt
```
To do this, type this command to open the dir.conf file in a text editor with root privileges:

```
sudo nano /etc/apache2/mods-enabled/dir.conf
```
```
<IfModule mod_dir.c>
    DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
</IfModule>
```
We want to move the PHP index file highlighted above to the first position after the DirectoryIndex specification, like this:

```
<IfModule mod_dir.c>
    DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
```

After this, we need to restart the Apache web server in order for our changes to be recognized. You can do this by typing this:

```
sudo service apache2 restart
```
If we decided that php5-cli is something that we need, we could type:
```
sudo apt-get install php5-cli
```

## Step 4: Test PHP Processing on your Web Server

In Ubuntu 16.04, this directory is located at `/var/www/html/`. We can create the file at that location by typing:

```
sudo nano /var/www/html/info.php
```
```
<?php
phpinfo();
?>
```
The address you want to visit will be:

```
http://your_server_IP_address/info.php
```
# How To Install and Secure phpMyAdmin on Ubuntu 16.04

## Step 1: — Install phpMyAdmin

```
sudo apt-get update
sudo apt-get install phpmyadmin php-mbstring php-gettext
```
```
sudo phpenmod mcrypt
sudo phpenmod mbstring
```
Add Phpmyadmin  to `/etc/apache2/apache2.conf`
```
# Include Phpmyadmin
ServerName ip.add.re.ss
Include /etc/phpmyadmin/apache.conf
```
```
sudo systemctl restart apache2
```

```
https://domain_name_or_IP/phpmyadmin
```
## Step 2: — Secure your phpMyAdmin Instance

### Configure Apache to Allow .htaccess Overrides
First, we need to enable the use of `.htaccess` file overrides by editing our Apache configuration file.

```
sudo nano /etc/apache2/conf-available/phpmyadmin.conf
```
```
/etc/apache2/conf-available/phpmyadmin.conf
```
```
<Directory /usr/share/phpmyadmin>
    Options FollowSymLinks
    DirectoryIndex index.php
    AllowOverride All
</Directory>
```
To implement the changes you made, restart Apache:
```
sudo systemctl restart apache2
```
### Create an .htaccess File

```
sudo nano /usr/share/phpmyadmin/.htaccess
```
Within this file, we need to enter the following information:
```
AuthType Basic
AuthName "Restricted Files"
AuthUserFile /etc/phpmyadmin/.htpasswd
Require valid-user
```
### Create the .htpasswd file for Authentication

The location that we selected for our password file was `"/etc/phpmyadmin/.htpasswd"`. We can now create this file and pass it an initial user with the htpasswd utility:
```
sudo htpasswd -c /etc/phpmyadmin/.htpasswd username
```
```
https://domain_name_or_IP/phpmyadmin
```
### change phpMyAdmin access url
from https://domain_name_or_IP/phpmyadmin to https://domain_name_or_IP/not-soknown-url

```
sudo nano /etc/phpmyadmin/apache.conf
```
Change this:
```
Alias /somthing_url_here /usr/share/phpmyadmin
```
