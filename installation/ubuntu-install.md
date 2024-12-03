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
sudo apt-get --no-install-recommends -y apache2 ghostscript libapache2-mod-php mysql-server php php-bcmath php-curl php-imagick php-intl php-json php-mbstring php-mysql php-xml php-zip
```

## **3.Security note: After installation, we will:**
- Secure your MySQL installation: 
```bash
sudo mysql_secure_installation
```
- Configure Apache with proper security headers
- Set appropriate file permissions
- Configure PHP with secure settings in php.ini
