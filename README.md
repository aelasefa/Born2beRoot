# 🔐 Born2beroot Project

This repository contains scripts and configurations for setting up a secure and functional **Debian-based virtual machine** for server management.  
It includes services that support **WordPress** hosting and a bonus service for easier system administration using **Webmin**.

---

## 📚 Table of Contents

- [About Born2beroot](#about-born2beroot)
- [Project Goals](#project-goals)
- [System Requirements](#system-requirements)
- [Setup Instructions](#setup-instructions)
- [WordPress Deployment](#wordpress-deployment)
- [Install Webmin (Bonus)](#install-webmin-bonus-service)
- [Services Included](#services-included)
- [Security Considerations](#security-considerations)
- [Resources](#resources)

---

## 🧠 About Born2beroot

**Born2beroot** is a system administration project designed to teach you how to:

- Set up a secure virtual machine with essential services.
- Deploy WordPress using **Lighttpd** and **MariaDB**.
- Integrate a web-based administration tool like **Webmin**.

This project boosts your understanding of web servers, databases, and Linux system management.

---

## 🎯 Project Goals

- ✅ Deploy **Lighttpd** as a lightweight web server for WordPress.
- ✅ Set up **MariaDB** as the database server.
- ✅ Install **Webmin** for bonus web-based system administration.
- ✅ Apply security best practices: sudo, firewall, SSH, and permissions.

---

## 💻 System Requirements

- Debian 12 (Bookworm) or compatible Debian-based system.
- VirtualBox / VMware for virtualization.
- Internet connection for installing packages.
- `sudo` privileges for installation and configuration.

---

## ⚙️ Setup Instructions

### 1. Update the System

```bash
sudo apt update && sudo apt upgrade -y
```

---

### 2. Install Lighttpd

```bash
sudo apt install lighttpd -y
sudo systemctl enable lighttpd
sudo systemctl start lighttpd
```

Verify it's running:

```bash
sudo systemctl status lighttpd
```

Allow HTTP traffic:

```bash
sudo ufw allow 80/tcp
```

---

### 3. Install MariaDB

```bash
sudo apt install mariadb-server -y
sudo systemctl enable mariadb
sudo systemctl start mariadb
```

Secure the installation:

```bash
sudo mysql_secure_installation
```

Create a database and user:

```sql
sudo mysql -u root -p
CREATE DATABASE wordpress;
CREATE USER 'wp_user'@'localhost' IDENTIFIED BY 'your_password';
GRANT ALL PRIVILEGES ON wordpress.* TO 'wp_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

---

### 4. Install PHP

```bash
sudo apt install php php-cgi php-mysql php-fpm -y
sudo systemctl restart lighttpd
```

---

## 📝 WordPress Deployment

1. Download and extract WordPress:

```bash
wget https://wordpress.org/latest.tar.gz
tar -xzvf latest.tar.gz
sudo mv wordpress /var/www/html/
```

2. Configure WordPress:

```bash
sudo cp /var/www/html/wordpress/wp-config-sample.php /var/www/html/wordpress/wp-config.php
sudo nano /var/www/html/wordpress/wp-config.php
```

Update database settings:

```php
define('DB_NAME', 'wordpress');
define('DB_USER', 'wp_user');
define('DB_PASSWORD', 'your_password');
define('DB_HOST', 'localhost');
```

3. Set permissions:

```bash
sudo chown -R www-data:www-data /var/www/html/wordpress
```

4. Open in browser:

```
http://<your-server-ip>/wordpress
```

---

## 🧰 Install Webmin (Bonus Service)

1. Add Webmin GPG key and repository:

```bash
wget -qO- http://www.webmin.com/jcameron-key.asc | sudo tee /etc/apt/trusted.gpg.d/webmin.asc
sudo sh -c 'echo "deb http://download.webmin.com/download/repository sarge contrib" > /etc/apt/sources.list.d/webmin.list'
```

2. Install Webmin:

```bash
sudo apt update
sudo apt install webmin -y
```

3. Access Webmin in browser:

```
https://<your-server-ip>:10000
```

Login using root credentials.

---

## 🔗 Services Included

| Service   | Purpose                             | Access                                 |
|-----------|-------------------------------------|----------------------------------------|
| Lighttpd  | Web server for WordPress            | `http://<server-ip>/wordpress`         |
| MariaDB   | WordPress database backend          | Access via terminal                    |
| Webmin    | Web admin panel (bonus)             | `https://<server-ip>:10000`            |

---

## 🔐 Security Considerations

- Use **strong passwords** for all services.
- Limit **Webmin** access to trusted IP addresses.
- Enable a firewall:

```bash
sudo ufw allow 80/tcp      # HTTP
sudo ufw allow 10000/tcp   # Webmin
sudo ufw enable
```

---

## 📚 Resources

- [WordPress Documentation](https://wordpress.org/support/)
- [Lighttpd Documentation](https://redmine.lighttpd.net/projects/lighttpd/wiki)
- [MariaDB Documentation](https://mariadb.com/kb/en/documentation/)
- [Webmin Documentation](https://doxfer.webmin.com/)
- Born2beroot Subject on Intra

---

> 🧩 *“Security is not a product, but a process. Born2beroot teaches you how to build it from the ground up.”*
