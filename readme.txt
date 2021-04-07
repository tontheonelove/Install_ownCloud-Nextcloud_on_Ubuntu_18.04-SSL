# Install_ownCloud-Nextcloud_on_Ubuntu_18.04-SSL


////////////////Install Apache///////////////

#apt-get update

#apt install apache2

#systemctl status apache2

#apt install software-properties-common
#add-apt-repository ppa:ondrej/php
#apt update

///////////////Install PHP 7.3///////////////

#apt install php7.3 libapache2-mod-php7.3 openssl php7.3-common php7.3-mysql php7.3-xml php7.3-xmlrpc php7.3-curl php7.3-gd php7.3-imagick php7.3-dev php7.3-imap php7.3-mbstring php7.3-zip php7.3-intl php7.3-json php-ssh2 php-apcu php-redis redis-server unzip -y

//////////////// Configure PHP 7.3 //////////////////

#nano /etc/php/7.3/apache2/php.ini

==ADD THIS==

upload_max_filesize = 64M 
post_max_size = 64M 
memory_limit = 256M 
max_execution_time = 600 
max_input_vars = 5000 
max_input_time = 1000


//////////////////  Install MariaDB Database Server  ///////////////////

#apt install mariadb-server mariadb-client

#systemctl start mariadb

#systemctl enable mariadb

#mysql_secure_installation

====ADD PASSWORD  AND ENTER Y UNTILL DONE ======



//////////////////// CREATE DATABASES FOR  OWNCLOUD  OR  NEXTCLOUD  ///////////////////////

#mysql -u root -p

create database nextcloud;

create user nextclouduser@localhost identified by 'your-password';

grant all privileges on nextcloud.* to nextclouduser@localhost identified by 'your-password';

flush privileges;

exit;



///////////////Download ownCloud Or Nextcloud /////////////

#wget https://download.owncloud.org/community/owncloud-10.3.2.zip (Owncloud)  Or  https://download.nextcloud.com/server/releases/nextcloud-21.0.0.zip (Nextcloud)

#unzip owncloud-10.3.2.zip Or nextcloud-21.0.0.zip  -d /var/www/html

#chmod -R 755 /var/www/html/owncloud
#chown -R www-data:www-data /var/www/html/owncloud




/////////////////   Configure Apache   /////////////////

#a2dissite 000-default

#nano /etc/apache2/sites-available/owncloud.conf  Or nextcloud.conf

/////////////  ADD THIS  //////////////

<VirtualHost *:80>
     ServerAdmin admin@domainname.com
     ServerName domainname.com
     ServerAlias www.domainname.com

     DocumentRoot /var/www/html/owncloud

     <Directory /var/www/html/owncloud>
         Options Indexes FollowSymLinks
         AllowOverride All
         Require all granted
     </Directory>

     ErrorLog ${APACHE_LOG_DIR}/error.log 
     CustomLog ${APACHE_LOG_DIR}/access.log combined 
 </VirtualHost>
 
 
 
//////////////Install Let’s Encrypt SSL///////////////////

#add-apt-repository ppa:certbot/certbot
#apt update
#apt install python-certbot-apache
#certbot --apache --agree-tos --redirect -m youremail@email.com -d domainname.com

Try tun Your Domain On Webbrowser  and Connect your Databases 


////////////// Renewing SSL Certificate //////////////////

#certbot renew --dry-run




