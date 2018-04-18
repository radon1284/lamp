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
