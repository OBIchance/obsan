+++
title = "IT Ticketing System Setup and Management"
date = "2024-12-28T22:08:43-05:00"
draft = false
author = "Obsan Muzemil"
updated = "2024-12-28"
summary = "A walkthrough of setting up and configuring osTicket to manage IT support requests."
tags = ["ticketing", "help desk", "osTicket", "projects"]
categories = ["Projects"]
+++
# Ticketing System Setup and Management

## Introduction
I set up and configured osTicket to manage IT support requests. This project simulates a help desk environment and demonstrates my skills in server management, problem-solving, and customer service.

I worked with Linux (Ubuntu) to install and run web servers, manage databases, and handle support tickets. This hands-on experience helped me better understand help desk operations and troubleshooting.

## Tools I Used
- VMware Workstation  
- Linux (Ubuntu/Debian)  
- osTicket  
- Apache, MySQL, PHP  

---

## Setting Up the Environment
For this project, I used VMware Workstation to create a virtual machine (VM). Since I already had VMware Workstation installed on my computer, I jumped straight to creating the VM. I set up Ubuntu on the VM to host the osTicket system.  

![VMware Workstation ](/images/Ticket/VM.webp)  

---

## Software Installation
I downloaded the Ubuntu Server ISO from the official website and installed it on the VM.  

![Ubuntu Installation](/images/Ticket/updating-ubuntu.png)  

To get the web server running, I installed Apache, MySQL, and PHP by running:
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install apache2 mysql-server php libapache2-mod-php php-cli php-mysql unzip
```
![Ubuntu Terminal Installation](/images/Ticket/LAMP.png)  

---

## Installing osTicket
After setting up the server, I downloaded and extracted the osTicket files directly into the web server directory:
```bash
cd /var/www/html
sudo wget https://github.com/osTicket/osTicket/releases/download/v1.17/osTicket-v1.17.zip
sudo unzip osTicket-v1.17.zip
sudo mv upload osticket
```

![osTicket File Extraction](/images/Ticket/downloading-ticket-system.gif)  

---

## Configuring Apache for osTicket
To ensure osTicket runs smoothly, I configured Apache by creating a virtual host file:
```bash
sudo nano /etc/apache2/sites-available/osticket.conf
```
I added the following configuration:
```
<VirtualHost *:80>
    ServerAdmin admin@example.com
    DocumentRoot /var/www/html/osticket
    ServerName your_domain_or_ip

    <Directory /var/www/html/osticket>
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
I enabled the site and restarted Apache:
```bash
sudo a2ensite osticket
sudo systemctl restart apache2
```

---

## Database Setup
To get osTicket working, I created a MySQL database and user by entering these commands:
```sql
CREATE DATABASE osticket;
CREATE USER 'osticket_user'@'localhost' IDENTIFIED BY '';
GRANT ALL PRIVILEGES ON osticket.* TO 'osticket_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```
![Database](/images/Ticket/mysql-setup.png)  

---

## Web Setup and Configuration
I accessed the osTicket setup by going to this address in my browser:
![osTicket Setup Wizard](/images/Ticket/Screenshot-2024-12-28-201538.png)  

```
http://<vm_ip>/osticket/setup/
```
From there, I created departments, help topics, and user roles.  

![osTicket ](/images/Ticket/osTIcket.png)  

### Fixing Missing Configuration File
During the setup, osTicket couldn’t find `ost-config.php`. I fixed this by adjusting the permissions and copying the configuration file:

![osTicket Setup ](/images/Ticket/issue-with-accessing-setup.png)
```bash
cd /var/www/html/osticket/include
sudo cp ost-sampleconfig.php ost-config.php
sudo chown www-data:www-data ost-config.php
sudo chmod 0666 ost-config.php
sudo systemctl restart apache2
```
  
---

## Testing the System
To make sure everything worked, I submitted and resolved sample tickets. This helped me confirm the system was running smoothly and allowed me to troubleshoot any issues that popped up.  

![Sample Ticket Submission](/images/Ticket/vmware_A3BNqXsyMk.gif)  

## Resolving tickets
Resolving tickets submited by users I was able to confirm that this works. 
![Sample Ticket ](/images/Ticket/vmware_aw5QpimRT2.gif)  




