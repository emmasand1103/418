# Project 2 using Fedora
<!--updating Fedora -->
sudo dnf update

<!--installing apache -->
sudo dnf install httpd
sudo systemctl start httpd
sudo systemctl enable httpd
sudo systemctl status httpd

sudo dnf install w3m
w3m localhost
w3m 34.31.135.93

cd /var/www/html/
sudo nano index.html
<!--IP address works and apache is installed and running -->

<!--installing php -->
sudo dnf install php php-common php-mysqlnd
sudo systemctl restart httpd
systemctl status httpd
cd /var/www/html/
sudo nano info.php

<!--configuring index.html-->
cd /etc/httpd/conf
sudo nano httpd.conf
<!--search for DirectoryIndex and added index.php before index.html-->
httpd -t
sudo systemctl restart httpd
cd /var/www/html
sudo nano index.php

<!--installing MariaDB -->
sudo dnf install mariadb-server 
sudo systemctl enable mariadb 
sudo systemctl start mariadb
systemctl status mariadb

<!--log in as root user-->
sudo su
mariadb -u root
create create user 'webapp'@'localhost' identified by 'XXXXXXXXX';
create database linuxdb;
grant all privileges on linuxdb.* to 'webapp'@'localhost';
show databases;
\q
exit
mariadb -u webapp -p
show databases;
use linuxdb;
create table distributions (id int unsigned not null auto_increment, name varchar(150) not null, developer varchar(150) not null, founded date not null, primary key (id));
show tables;
describe distributions;
insert into distributions (name, developer, founded) values ('Debian', 'The Debian Project', '1993-09-15'), ('Ubuntu', 'Canonical Ltd.', '2004-10-20'), ('Fedora', 'Fedora Project', '2003-11-06');
atler table distributions add packagemanager char (3) after name;
update distributions set packagemanager='APT' where id='1';
update distributions set packagemanager='APT' where id='2';
update distributions set packagemanager='DNF' where id='3';
\q
exit

<!--pho modules for wordpress-->
sudo dnf install php-xml php-json php-gd php-mbstring
sudo systemctl restart httpd
sudo systemctl restart mariadb
cd /var/www/html/
sudo touch login.php
sudo chmod 640 login.php
sudo chown :apache login.php
ls -l login.php
sudo nano login.php
sudo nano distros.php

<!--install wordpress-->
sudo dnf install php-fpm php-mysqlnd php-opcache php-gd \ php-xml php-mbstring php-curl php-pecl-imagick php-pecl-zip libzip
sudo systemctl restart httpd
sudo systemctl restart mariadb

<!--install wget command for Fedora-->
sudo dnf update
sudo dnf search wget
sudo dnf install wget

sudo wget https://www.wordpress.org/latest.tar.gz
sudo tar -xvf latest.tar.gz 

<!--create database and users for wordpress-->
sudo su
mariadb -u root
create user 'wordpress'@'localhost' identified by 'XXXX'
create database wordpress;
grant all privileges on wordpress.* to 'wordpress'@'localhost';
show databases;
\q

cd wordpress
sudo cp wp-config-sample.php wp-config.php
sudo nano wp-config.php
<!--entered credentials and disabled FTP -->
Sudo chown -R apache:apache /var/www/html/wordpress

<!--http://34.31.135.93/wordpress/ now functions-->
