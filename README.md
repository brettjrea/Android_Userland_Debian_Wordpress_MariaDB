# Android_Userland_Debian_Wordpress_MariaDB

A setup script and breakdown to install Wordpress CMS with MariaDB on Android using Debian 10 'Buster' Linux in the [Userland App](https://github.com/CypherpunkArmory/UserLAnd). Haven't tested it on Debian 10 'Buster' for WSL or a VM but probably would work. Docker definitely needs docker specific commands

# Installing quickly with a script.

```
sudo apt update && sudo apt upgrade -y
sudo apt autoremove -y
sudo apt install wget -y
wget https://raw.githubusercontent.com/brettjrea/Android_Userland_Debian_Wordpress_MariaDB/master/setupwp.sh
bash setupwp.sh
```

### Install by doing what the script does snip by snip.

### Update, upgrade & clean.

```
sudo apt update && sudo apt upgrade -y
```

`sudo apt autoremove -y`

### Install MariaDB and PHP.

```
sudo apt install mariadb-server php-cli php-curl php-gd php-intl php-mbstring php-mysql php-soap php-xml php-xmlrpc php-zip wget -y`
```
### Start Mysql & Create User.

`sudo service mysql start`

```
sudo mysql -uroot <<_EOF_ 
CREATE USER 'dbuser'@'localhost' IDENTIFIED BY 'dbpass';
GRANT ALL PRIVILEGES ON *.* TO 'dbuser'@'localhost';
FLUSH PRIVILEGES;
_EOF_
```
### Make WP directory.

`mkdir /wp/`

### Change into WP directory.

`cd /wp/`

### Download WP-CLI.deb.

```
wget -O "wpcli.deb" 'https://github.com/wp-cli/builds/raw/gh-pages/deb/php-wpcli_2.4.0_all.deb'
sudo apt install ./wpcli.deb
rm -f wpcli.deb
```
### Update WP-CLI.

`wp cli update`

### Use WP-CLI to download wordpress to /wp.

`wp core download --path=/wp/`

### Use WP-CLI to Create config.php

```
wp config create --path=/wp/ --dbhost=localhost --dbname=wordpress --dbuser=dbuser --dbpass=dbpass`
```
### Use WP-CLI to create database.

`wp db create --path=/wp/`

### Use WP-CLI to run install.

```
wp core install --path=/wp/ --url=http://localhost:8080/ --title=wordpress --admin_user=wpuser --admin_password=wppass --admin_email=info@example.com
```

### Start PHP built-in webserver on port 3000.

`
wp server
`
