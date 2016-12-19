



# switch to the root user (to avoid typing sudo)
sudo su -

# install apache
apt-get -y install apache2

# install fastcgi apache module + php-fpm 
apt-get -y install libapache2-mod-fastcgi php7.0-fpm php7.0


# add our site to the apache config
cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/nume.getdisplaywall.com.conf
nano /etc/apache2/sites-available/nume.getdisplaywall.com.conf

		#ServerName www.example.com
		->
		ServerName nume.getdisplaywall.com

		ServerAdmin webmaster@localhost
		->
		ServerAdmin email@devpadawan.ro


		DocumentRoot /var/www/html
		->
		DocumentRoot /var/www/html/nume.getdisplaywall.com

# create the directory where site files are stored
mkdir /var/www/html/nume.getdisplaywall.com

# create a test page for php
echo "<?php phpinfo();" > /var/www/html/nume.getdisplaywall.com/phpinfo.php

# add the correct owner to the files
chown -R www-data:www-data /var/www/html/nume.getdisplaywall.com/

# enable the site in apache config
a2ensite nume.getdisplaywall.com.conf

# enable the fpm apache config
a2enconf php7.0-fpm.conf

# enable the fastcgi apache module
a2enmod proxy_fcgi

# restart apache to load the new changes
service apache2 restart

##
## now you can visit the site http://nume.getdisplaywall.com/phpinfo.php (after the client hosts file has been edited to resolve nume.getdisplaywall.com to the server IP)
##

##########################################################################
## Chaning the apache ports to listen on port 81 (http) and 444 (https) ##

nano /etc/apache2/ports.conf

	Listen 80
	->
	Listen 81

	Listen 443
	->
	Listen 444

nano /etc/apache2/sites-available/nume.getdisplaywall.com.conf
	<VirtualHost *:80>
	->
	<VirtualHost *:81>
	
nano /etc/apache2/sites-available/000-default.conf
	<VirtualHost *:80>
	->
	<VirtualHost *:81>

nano /etc/apache2/sites-available/default-ssl.conf
	<VirtualHost _default_:443>
	->
	<VirtualHost _default_:444>

## restart apache to load the new changes
service apache2 restart


## install nginx
apt-get install nginx

## add our domain to nginx config folders
touch /etc/nginx/sites-available/nume.getdisplaywall.com.conf

## configure the site
Edit nume.getdisplaywall.com.conf to contain something along the lines of https://github.com/tmariovlad/nginx-proxy-httpd-php-fpm/blob/master/nume.getdisplaywall.com.conf

## activate the site in the nginx config
ln -s /etc/nginx/sites-available/nume.getdisplaywall.com.conf /etc/nginx/sites-enabled/nume.getdisplaywall.com.conf

## restart nginx to load the new changes
service nginx restart

##
## now you can visit the site http://nume.getdisplaywall.com/phpinfo.php (after the client hosts file has been edited to resolve nume.getdisplaywall.com to the server IP)
##
