# Installation Guide: WordPress on Ubuntu

This guide walks you through installing and configuring WordPress on an Ubuntu server.

---

## **1. Update and Upgrade Your System**
Whilst this guide is to guide you on installing and configuring wordpress via Ubuntu you will need to build a base box:

The below links explain how to get this up and running at time of writing (some links and versions may have changed):

**You will be required to create an HashiCorp account to access the boxs**

Box build understanding: https://developer.hashicorp.com/vagrant/docs/boxes/base /n
Vitual box build: https://developer.hashicorp.com/vagrant/docs/providers/virtualbox/boxes /n
Box version used: https://portal.cloud.hashicorp.com/vagrant/discover/ubuntu/focal64

## **2. Update and Upgrade Your System**
Run the following commands to ensure your system is up to date:

```bash
sudo apt update && sudo apt upgrade -y

