<!-- base_template: frappe_io/www/frappe/frappe_base.html -->
<!-- add-breadcrumbs -->
# Installation

Frappe installation involves certain steps according to the Operating system and dependencies:

1. [Installing Pre-requisites](#pre-requisites)
1. [Installing Bench CLI](#bench)
1. [Initialising Frappe Bench and start the site](#initialise-bench-and-start-the-site)
1. [Creating a new site](#creating-a-new-site)
1. [Installing Existing App (ERPNext Example)](#installing-existing-app)

--- 

## Pre-requisites for Frappe Environment

### Pre-requisites

Frappe requires a set of requirements and dependencies for setting up development and production environmnent. Some of the higher level pre-requisites are:

1. Python 2.7 (Python 3.5+ also supported)
1. MariaDB 10+
1. Nginx (for production)
1. Git
1. Nodejs
1. yarn
1. Redis
1. cron (crontab is required)
1. wkhtmltopdf with patched Qt (version 0.12.3) (for pdf generation)

### Installing pre-requisistes in different platforms


> These steps assume you want to install Bench in developer mode. If you want install in production mode, follow the [Easy Install](https://github.com/frappe/bench#easy-install) method. This method will also take care of installing the pre-requisites according to supported operating system.

This section will guide you through installation of requirements in different platforms:


1. [MacOS](#macos)
1. [Debian / Ubuntu](#debian-ubuntu)
1. [Arch Linux](#arch-linux)
1. CentOS

After you have installed the pre-requisites in your platform, its time to [install bench](#bench)). 

> This guide assumes you are using a personal computer, VPS or a bare-metal server. You also need to be on a *nix system, so any Linux Distribution and MacOS is supported. However, we officially support only the following distributions.

 

### MacOS

Install [Homebrew](https://brew.sh/). It makes it easy to install packages on macOS.

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Now, you can easily install the required packages by running the following command

```bash
brew install python
brew install git
brew install redis
brew install mariadb

brew tap caskroom/cask
brew cask install wkhtmltopdf
```

**Install Node**

We recommend installing node using [nvm](https://github.com/creationix/nvm)

```bash
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash
```

After nvm is installed, you may have to close your terminal and open another one. Now run the following command to install node.

```bash
nvm install 8
```

Verify the installation, by running:

```bash
node -v
# output
v8.11.3
```

Finally, install yarn using npm

```bash
npm install -g yarn
```

### Debian / Ubuntu

Install `git`, `python`, and `redis`

```bash
sudo apt install git python-dev redis-server
```

**Install MariaDB**

```bash
sudo apt-get install software-properties-common
sudo apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xF1656F24C74CD1D8
sudo add-apt-repository 'deb [arch=amd64,i386,ppc64el] http://ftp.ubuntu-tw.org/mirror/mariadb/repo/10.3/ubuntu xenial main'
sudo apt-get update
sudo apt-get install mariadb-server-10.3
```

During this installation you'll be prompted to set the MySQL root password. If you are not prompted, you'll have to initialize the MySQL server setup yourself. You can do that by running the command:

```bash
mysql_secure_installation
```

> Remember: only run it if you're not prompted the password during setup.

It is really important that you remember this password, since it'll be useful later on. You'll also need the MySQL database development files.

```bash
sudo apt-get install libmysqlclient-dev
```

Now, edit the MariaDB configuration file.

```bash
sudo nano /etc/mysql/my.cnf
```

And add this configuration

```hljs
[mysqld]
character-set-client-handshake = FALSE
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci

[mysql]
default-character-set = utf8mb4
```

Now, just restart the mysql service and you are good to good

```bash
sudo service mysql restart
```

**Install Node**

We recommend installing node using [nvm](https://github.com/creationix/nvm)

```bash
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash
```

After nvm is installed, you may have to close your terminal and open another one. Now run the following command to install node.

```bash
nvm install 8
```

Verify the installation, by running:

```bash
node -v
# output
v8.11.3
```

Finally, install `yarn` using `npm`

```bash
npm install -g yarn
```

**Install wkhtmltopdf**

```
sudo apt-get install xvfb libfontconfig wkhtmltopdf
```

### Arch Linux

Install packages using pacman

```bash
pacman -Syu
pacman -S mariadb redis python2-pip wkhtmltopdf git npm cronie nginx openssl
npm install -g yarn
```

Setup MariaDB

```bash
mysql_install_db --user=mysql --basedir=/usr --datadir=/var/lib/mysql
systemctl start mariadb
mysql_secure_installation
```

Edit the MariaDB configuration file

```bash
nano /etc/mysql/my.cnf
```

Add the following configuration

```
[mysqld]
innodb-file-format=barracuda
innodb-file-per-table=1
innodb-large-prefix=1
character-set-client-handshake = FALSE
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci

[mysql]
default-character-set = utf8mb4
```

Start services

```bash
systemctl start mariadb redis
```

If you don't have cron service enabled you would have to enable it.

```bash
systemctl enable cronie
```

[Top](#installation)

--- 

## Bench


### What is Bench ?
To install Frappe, you use Bench CLI. The Bench is a 

- command-line utility 
- that helps you to install apps 
- manage multiple sites and 
- update Frappe / ERPNext apps  
- Bench will also create nginx and supervisor config files, setup backups and much more.

> Learn more about the architecture [here](/docs/installation/architecture).


You will have to install Bench first in order to install Frappe and start developing Frappe Apps.


### Install Bench

Install bench as a non-root user

```bash
git clone https://github.com/frappe/bench bench-repo
pip install --user -e bench-repo
```

Confirm the bench installation by checking version

```bash
bench --version

# output
4.1.0
```

```bash
bench start
```

Congratulations, you have installed bench on to your system.


---

## Initialise Bench 

Now when the bench is installed, you will have to initialise it to have a folder structure which will hold multi-tenant Frappe Applications.

You will use following Bench command to initialise the bench:

```bash
cd ~
bench init frappe-bench
```

After the frappe-bench folder is created, change your directory to frappe-bench.

```bash
cd frappe-bench
```

[Top](#installation)

---

## Creating a new site

You will have to create a new site and install Frappe and in case you have one, install your custom app, to initialised Frappe Bench.

```bash
bench new-site site1.local
```

It will ask for mysql root password and generate folder structure of a new site. It will also ask for administrator password for the new site. 

Here `site1.local` is the name of the site. You can choose your site name as per your scenario. 

You can also make an entry of this sitename in your etc.hosts for mapping the site name with current host:

```
...
127.0.0.1   localhost

# add this line
127.0.0.1   site1.local
...
```

[Top](#installation)

---

## Installing Existing App

### Getting the remote apps
You can get any remote frappe apps by using the get-app command from remote git repository. Example: [ERPNext](https://github.com/frappe/erpnext)

```bash
  bench get-app erpnext https://github.com/frappe/erpnext
```

### Installing the apps to your site

To install an app on your new site, use the bench install-app command.

```  bash
bench --site site1.local install-app erpnext
```

### Start bench
To start using the bench, use the bench start command

```bash
bench start
```

To login to Frappe / ERPNext, open your browser and go to [your-external-ip]:8000, probably localhost:8000

The default username is "Administrator" and password is what you set when you created the new site.

[Top](#installation)
