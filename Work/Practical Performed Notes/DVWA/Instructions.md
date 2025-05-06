So the latest version's github repo is already given and also installation steps are given.

I have to install these things before getting started
```bash
sudo apt update
sudo apt install -y apache2 mariadb-server mariadb-client php php-mysqli php-gd libapache2-mod-php
```
Then we have to do is create a database from the page itself and details are also given there on the instructions page.

so first we will be doing is create database and do the setup thing

```mysql
#The variables are set to the following by default:

$_DVWA[ 'db_server'] = '127.0.0.1';
$_DVWA[ 'db_port'] = '3306';
$_DVWA[ 'db_user' ] = 'dvwa';
$_DVWA[ 'db_password' ] = 'p@ssw0rd';
$_DVWA[ 'db_database' ] = 'dvwa';
```
then the database checking in the kali so we will be doing this
the instructions .................................
![](https://i.imgur.com/5Th0AbA.png)


![](https://i.imgur.com/H5pa8Ag.png)




### Steps to install dvwa latest one:
To set up **Damn Vulnerable Web Application (DVWA)** on **Kali Linux**, follow these steps:

---

## **Step 1: Install Required Packages**

Before setting up DVWA, ensure you have **Apache**, **MariaDB**, and **PHP** installed:

bash

CopyEdit

```bash
sudo apt update && sudo apt upgrade -y 
sudo apt install apache2 mariadb-server php php-mysqli php-gd php-curl -y
```

Enable and start Apache and MariaDB:

bash

CopyEdit

```bash
sudo systemctl enable apache2 mariadb sudo systemctl start apache2 mariadb
```


---

## **Step 2: Download DVWA**

Clone the DVWA repository from GitHub:

bash

CopyEdit

```bash
cd /var/www/html sudo
git clone https://github.com/digininja/DVWA.git
```


Set proper permissions:

bash

CopyEdit

```bash
sudo chmod -R 777 /var/www/html/DVWA 
sudo chown -R www-data:www-data /var/www/html/DVWA
```


Rename the configuration file:

bash

CopyEdit

```bash
cd /var/www/html/DVWA/config 
sudo cp config.inc.php.dist config.inc.php
```


---

## **Step 3: Configure Database**

Start MySQL and secure it:

bash

CopyEdit

```bash
sudo mysql_secure_installation
```


- Press **Enter** for root password (default is empty).
- Set a new root password.
- Answer **Y** (Yes) to all security prompts.

Log in to MySQL:

bash

CopyEdit

`sudo mysql -u root -p`

Create the **dvwa** database and user:

sql

CopyEdit

`CREATE DATABASE dvwa; CREATE USER 'dvwa'@'localhost' IDENTIFIED BY 'password'; GRANT ALL PRIVILEGES ON dvwa.* TO 'dvwa'@'localhost'; FLUSH PRIVILEGES; EXIT;`

---

## **Step 4: Update DVWA Configuration**

Edit the **config.inc.php** file:

bash

CopyEdit

`sudo nano /var/www/html/DVWA/config/config.inc.php`

Find and update these lines:

php

CopyEdit

`$_DVWA[ 'db_user' ] = 'dvwa'; $_DVWA[ 'db_password' ] = 'password'; $_DVWA[ 'db_database' ] = 'dvwa';`

Save the file (`CTRL + X`, then `Y`, and press `ENTER`).

---

## **Step 5: Enable Apache and Restart Services**

bash

CopyEdit

`sudo systemctl restart apache2 sudo systemctl restart mariadb`

---

## **Step 6: Access DVWA**

Open a browser and go to:

arduino

CopyEdit

`http://localhost/DVWA`

Click **"Create / Reset Database"** to set up the database.

### **Default Credentials:**

- **Username:** `admin`
- **Password:** `password`

Now, you have **DVWA running on Kali Linux**!

---

## **Optional: Enable Low Security Mode**

To test vulnerabilities, change **security level**:

1. Log in to DVWA.
2. Go to **DVWA Security**.
3. Set Security Level to **Low**.
4. Click **Submit**.

Now you're ready to practice **web security testing**! 🚀