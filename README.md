# Frappe-Installation-Guide
Frappe and ERPNext Installation guide



# ERP Setup and Installation Guide - version 15

This guide outlines the steps to follow while setting up ERPNext on Ubuntu 22.0.4 LTS.

**Table of content:**
- [Requirements](#requirements)
- [Steps](#steps)
- [wrap up](#wrap_up)



<!-- headings -->
<a id="requirements"></a>
## Requirements.
<ul>
    <li>
        Python version 3.11
    </li>
    <li>
        NodeJs version 18+
    </li>
    <li>
        Redis version 5+
    </li>
    <li>
        Yarn version 1.20+
    </li>
    <li>
        Python pip version 20+
    </li>
    <li>
        wkhtmltopdf version 0.12.6
    </li>
    <li>
        Bench version 5.20.0 +
    </li>
</ul>


<!-- Steps -->
<a id="steps"></a>
## Step 1.
### Install git if not installed.
```bash
sudo apt-get install git
``` 

## Step 2.
### Install python-dev
```bash
sudo apt-get install python3-dev
```

## Step 3
### Install python3 setup tools and pip package
```bash
sudo apt-get install python3-setuptools python3-pip
```

## Step 4.
### Install virtualenv
```bash
sudo apt-get install virtualenv
sudo apt install python3.11-venv
```


## Step 5.
### Install Maria DB
```bash
sudo apt-get install software-properties-common
sudo apt install mariadb-server
sudo mysql_secure_installation


In order to log into MariaDB to secure it, we'll need the current
password for the root user. If you've just installed MariaDB, and
haven't set the root password yet, you should just press enter here.

Enter current password for root (enter for none): # PRESS ENTER
OK, successfully used password, moving on...


Switch to unix_socket authentication [Y/n] Y
Enabled successfully!
Reloading privilege tables..
... Success!

Change the root password? [Y/n] Y
New password: 
Re-enter new password: 
Password updated successfully!
Reloading privilege tables..
... Success!

Remove anonymous users? [Y/n] Y
... Success!

Disallow root login remotely? [Y/n] Y
... Success!

Remove test database and access to it? [Y/n] Y
- Dropping test database...
... Success!
- Removing privileges on test database...
... Success!

Reload privilege tables now? [Y/n] Y
... Success!
```

## Step 6.
### Install MYSQL development tools.
```bash
sudo apt-get install libmysqlclient-dev
```

## Step 7.
### Install Edit MariaDB Configuration(unicode character encoding)
Open the mariadb configuartion file.
```bash
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
```

On the file add the following lines.
```bash
[server] ## Add this on this below [server] line.
user = mysql
pid-file = /run/mysqld/mysqld.pid
socket = /run/mysqld/mysqld.sock
basedir = /usr
datadir = /var/lib/mysql
tmpdir = /tmp
lc-messages-dir = /usr/share/mysql
bind-address = 127.0.0.1
query_cache_size = 16M
log_error = /var/log/mysql/error.log

[mysqld] ## Add this on this below [mysqld] line.
innodb-file-format=barracuda
innodb-file-per-table=1
innodb-large-prefix=1
character-set-client-handshake = FALSE
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci      
 
[mysql]   ## Add to the bottom of the file.
default-character-set = utf8mb4
```


Save the file, close and restart mysql server.

```bash
sudo service mysql restart

```
## Step 8.
### Install Redis server
```bash
sudo apt-get install redis-server
```

## Step 9.
### Install NodeJS 
```bash
sudo apt install curl 
curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash
source ~/.profile
nvm install [version] # version NodeJs Version 18+ LTS
```

## Step 10.
### Install NPM and Yarn Package manager.
```bash
sudo apt-get install npm

sudo npm install -g yarn
```


## STep 11.
### Install wkhtmltopdf package
```bash
sudo apt-get install xvfb libfontconfig wkhtmltopdf
```

## Step 12.
### Install frappe bench
```bash
sudo -H pip3 install frappe-bench==5.20.0
```

## Step 13
### Initialize bench and install frappe-bench latest version
```bash
bench init frappe-bench --frappe-branch version-15 # Change version to the latest relsease.
```
Change directory to bench and start bench
```bash
cd frappe-bench/

bench start # Start bench server served at port 0.0.0.0:80000
```

## Step 14.
### Create new site in frappe bench
Note: You should be in ``frappe_bench`` directory

```bash
bench new-site erp.co.ke

bench use erp.co.ke
```

## Step 15.
### Install ERPNext latest version in bench and site.

```bash
bench get-app erpnext --branch version-[version] # i.e version-[15]

# Download more other apps you might want.

# HR Module
bench get-app hrms

#Payments Module
bench get-app payments 
```

### Install the apps downloaded above.
```bash
# Install erpnext
bench --site  erp.co.ke install-app erpnext

# Install hrms
bench --site  erp.co.ke install-app hrms

# Install payments module if needed.
bench --site  erp.co.ke install-app payments
```


## Step 16.
### Start the server

```bash
bench start
```

If you encounter an error redis-queue connection error while installing any app, start bench and open new terminal instance inside frappe_bench directory then run bench migrate.

```bash
Please make sure that Redis Queue runs @ redis://127.0.0.1:11000. Redis reported error: Error 111 connecting to 127.0.0.1:11000. Connection refused.
Please make sure that Redis Queue runs @ redis://127.0.0.1:11000. Redis reported error: Error 111 connecting to 127.0.0.1:11000. Connection refused.
Please make sure that Redis Queue runs @ redis://127.0.0.1:11000. Redis reported error: Error 111 connecting to 127.0.0.1:11000. Connection refused.
Please make sure that Redis Queue runs @ redis://127.0.0.1:11000. Redis reported error: Error 111 connecting to 127.0.0.1:11000. Connection refused.
Please make sure that Redis Queue runs @ redis://127.0.0.1:11000. Redis reported error: Error 111 connecting to 127.0.0.1:11000. Connection refused
```

```bash 
cd frappe_bench 

# Make sure bench start is running on a separate terminal instance
bench migrate
```
 

Then re-run the install app command
```bash
bench --site [sitename] install-app [app]
#i.e bench --site erp.co.ke install-app hrms
```



<!-- wrapping up -->
<a id="wrap"></a>
## Wrapping up.

To run bench on the server and exit without interrupting the server, you can use `nohup`
```bash
nohup banch start &
```

You have successfully set up the erp.

## END.
