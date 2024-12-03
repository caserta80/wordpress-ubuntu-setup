# Installation Guide: WordPress on Ubuntu

This guide walks you through installing and configuring WordPress on an Ubuntu server.

---

## **1. Update and Upgrade Your System**
Whilst this guide is to guide you on installing and configuring wordpress via Ubuntu you will need to build a base box:

The below links explain how to get this up and running at time of writing (some links and versions may have changed):

**You will be required to create an HashiCorp account to access the boxs**

- Box build understanding: (https://developer.hashicorp.com/vagrant/docs/boxes/base)
- Vitual box build: (https://developer.hashicorp.com/vagrant/docs/providers/virtualbox/boxes)
- Box version used: (https://portal.cloud.hashicorp.com/vagrant/discover/ubuntu/focal64)

## **2. Update and Upgrade Your System**
Run the following commands to ensure your system is up to date:

```bash
sudo apt-get update && sudo apt upgrade -y
sudo apt-get install -y apache2 ghostscript libapache2-mod-php mysql-server php php-bcmath php-curl php-imagick php-intl php-json php-mbstring php-mysql php-xml php-zip

sudo mkdir -p /srv/www
sudo chown www-data: /srv/www
curl https://wordpress.org/latest.tar.gz | sudo -u www-data tar zx -C /srv/www
```
---

## **3. Create Apache site for WordPress.**
```bash
vim /etc/apache2/sites-available/wordpress.conf 
```
Add the following lines to the vim file:

<VirtualHost *:80>
    DocumentRoot /srv/www/wordpress
    <Directory /srv/www/wordpress>
        Options FollowSymLinks
        AllowOverride Limit Options FileInfo
        DirectoryIndex index.php
        Require all granted
    </Directory>
    <Directory /srv/www/wordpress/wp-content>
        Options FollowSymLinks
        Require all granted
    </Directory>
</VirtualHost>

---

## **Enable the site with the first cmd then Enable URL rewriting with second cmd and finally Disable the default “It Works” site with final cmd.
```bash
sudo a2ensite wordpress
sudo a2enmod rewrite
sudo a2dissite 000-default
```
---
## **Finally, reload apache2 to apply all these changes:
```bash
sudo service apache2 reload
```
## **To view changes all these changes:
```bash
ls -l /etc/apache2/sites-enabled/
output: lrwxrwxrwx 1 root root 33 Dec  3 13:48 wordpress.conf -> ../sites-available/wordpress.conf
ls -l /etc/apache2/sites-available/
```
## **4. Install & Configure MySQL**
```bash
sudo mysql -u root
CREATE DATABASE wordpress;
SHOW databases;
CREATE USER wordpress@localhost IDENTIFIED BY 'Hp7S74';
GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP,ALTER ON wordpress.* TO wordpress@localhost;
FLUSH PRIVILEGES;
quit;
sudo service mysql start
```

## **Security note: We will:**
- Secure your MySQL installation: 
- This program enables you to improve the security of your MySQL installation in the following ways:
- You can set a password for root accounts.
- You can remove root accounts that are accessible from outside the local host.
- You can remove anonymous-user accounts.
- You can remove the test database (which by default can be accessed by all users, even anonymous users), and privileges that permit anyone to access databases with names that start with test_.
```bash
sudo mysql_secure_installation
```
## **Later actions:
- Configure Apache with proper security headers
- Set appropriate file permissions
- Configure PHP with secure settings in php.ini

## **5.Now, let’s configure WordPress to use this database.**
## **First, copy the sample configuration file to wp-config.php:
```bash
sudo -u www-data cp /srv/www/wordpress/wp-config-sample.php /srv/www/wordpress/wp-config.php
```
Next, set the database credentials in the configuration file (do not replace database_name_here or username_here in the commands below. Do replace <your-password> with your database password.):
```bash
sudo -u www-data sed -i 's/database_name_here/wordpress/' /srv/www/wordpress/wp-config.php
sudo -u www-data sed -i 's/username_here/wordpress/' /srv/www/wordpress/wp-config.php
sudo -u www-data sed -i 's/password_here/Hp7S74/' /srv/www/wordpress/wp-config.php
```
Finally, in a terminal session open the configuration file in nano:
```bash
sudo vim /srv/www/wordpress/wp-config.php
```
Find the following:

- define( 'AUTH_KEY',         'put your unique phrase here' );
- define( 'SECURE_AUTH_KEY',  'put your unique phrase here' );
- define( 'LOGGED_IN_KEY',    'put your unique phrase here' );
- define( 'NONCE_KEY',        'put your unique phrase here' );
- define( 'AUTH_SALT',        'put your unique phrase here' );
- define( 'SECURE_AUTH_SALT', 'put your unique phrase here' );
- define( 'LOGGED_IN_SALT',   'put your unique phrase here' );
- define( 'NONCE_SALT',       'put your unique phrase here' );

Delete those lines (ctrl+k will delete a line each time you press the sequence). Then replace with the content of https://api.wordpress.org/secret-key/1.1/salt/. (This address is a randomiser that returns completely random keys each time it is opened.) This step is important to ensure that your site is not vulnerable to “known secrets” attacks.

Save and close the configuration file.
