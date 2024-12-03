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

## **Create Apache site for WordPress. Create /etc/apache2/sites-available/wordpress.conf with following lines:
```bash
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
```
---

## **Enable the site with:
```bash
sudo a2ensite wordpress
```
---

## **Enable URL rewriting with:
```bash
sudo a2enmod rewrite
```
---

## **Disable the default “It Works” site with:
```bash
sudo a2dissite 000-default
```
---

## **Or, instead of disabling the “it works” page, you may edit our configuration file to add a hostname that the WordPress installation will respond to requests for. This hostname must be mapped to your box somehow, e.g. via DNS, or edits to the client systems’ /etc/hosts file (on Windows the equivalent is C:\Windows\System32\drivers\etc\hosts). Add ServerName as below:

```bash
<VirtualHost *:80>
    ServerName hostname.example.com
    ... # the rest of the VHost configuration
</VirtualHost>
```
---

## **Finally, reload apache2 to apply all these changes:
```bash
sudo service apache2 reload
```

## **3.Security note: After installation, we will:**
- Secure your MySQL installation: 
```bash
sudo mysql_secure_installation
```
- Configure Apache with proper security headers
- Set appropriate file permissions
- Configure PHP with secure settings in php.ini
