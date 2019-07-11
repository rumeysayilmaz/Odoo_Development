## Documentation for Setting up Odoo 

This tutorial covers the steps needed for installing and configuring Odoo 12 Community version from Git source on a Python virtual environment on an _Ubuntu 18.04_ system. The steps for Odoo installation were utilized from [here](https://linuxize.com/post/how-to-deploy-odoo-12-on-ubuntu-18-04/) and [here](https://tecadmin.net/install-postgresql-server-on-ubuntu/) for PostgreSQL to create this instruction set.

### Updating the System to Latest Packages
1. Open up the Linux terminal as a sudo user and execute the commands below in a step by step manner:

```sh
$ sudo apt-get update
$ sudo apt-get upgrade
```

### Installing Odoo Dependencies
2. Install git, pip, [node.js](https://nodejs.org/) and tools required to build odoo dependencies:

```sh
$ sudo apt install git python3-pip build-essential wget python3-dev python3-venv python3-wheel libxslt-dev libzip-dev libldap2-dev libsasl2-dev python3-setuptools node-less
```

### Creating a System User for Odoo
3. Create a new system user named ```odoo12``` with a home directory ```/opt/odoo12``` by:

```sh
$ sudo useradd -m -d /opt/odoo12 -U -r -s /bin/bash odoo12
```

### Enabling PostgreSQL Apt Repository, Installing and Configuring PostgreSQL 
4. Enable PostgreSQL Apt Repository by importing the GPG key for PostgreSQL packages:

```sh
$ sudo apt-get install wget ca-certificates 
$ wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
```

5. Add the repository to your system:

```sh
$ sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'
```

6. Refresh the local package index and install the PostgreSQL server with PostgreSQL contribution package:

```sh
$ sudo apt update
$ sudo apt install postgresql postgresql-contrib
```

7. Verify PostgreSQL installation by running the following command:

```sh
$ sudo -u postgres psql -c "SELECT version();"
```

8. Creating a PostgreSQL user for the ```odoo12``` user:

```sh
$ sudo su - postgres -c "createuser -s odoo12"
```

>
>Make sure that the name of PostgreSQL username is same as the Odoo system user.
>


### Installing Wkhtmlopdf tool

9. Download the ```wkhtmlox``` package by employing wget command:

```sh
$ wget https://builds.wkhtmltopdf.org/0.12.1.3/wkhtmltox_0.12.1.3-1~bionic_amd64.deb
```

10. Install the ```wkhtmlox``` package:

```sh
$ sudo apt install ./wkhtmltox_0.12.1.3-1~bionic_amd64.deb
```

### Installing and Configuring Odoo
11. Switch to ```odoo12``` user:

```sh
$ sudo su - odoo12
```

12. Clone Odoo 12 community version from Git source:

```sh
$ git clone https://www.github.com/odoo/odoo --depth 1 --branch 12.0 /opt/odoo12/odoo
```

13. Creating a new Python Virtual Environment:

```sh
$ cd /opt/odoo12
$ python3 -m venv odoo-venv
```

14. Activate the virtual environment:

```sh
$ source odoo-venv/bin/activate
```

15. Install all of the required Python modules with pip3:

```sh
(venv) $ pip3 install wheel
(venv) $ pip3 install -r odoo/requirements.txt
```
>
>İf python3 version is higher than 3.7 then the PILLOW version inside odoo/requirements.txt need to be changed from 4.0.0 to the supported version. In my case it was 6.0.0. 
>

16. Deactivate the virtual environment:

```sh
(venv) $ deactivate
```

17. Create a new directory for custom addons:

```sh
$ mkdir /opt/odoo12/odoo-custom-addons
```

18. Switch back to sudo user:

```sh
$ exit
```

19. Create a configuration file:

```sh
$ sudo cp /opt/odoo12/odoo/debian/odoo.conf /etc/odoo12.conf
```

20. Open the configuration file:

```sh
$ sudo nano /etc/odoo12.conf
```

21. Edit the configuration file as:

```sh
[options]
; This is the password that allows database operations:
admin_passwd = enter-your-password
db_host = False
db_port = False
db_user = odoo12
db_password = False
addons_path = /opt/odoo12/odoo/addons,/opt/odoo12/odoo-custom-addons
```
>
>Make sure to change the ```enter-your-password``` indicated above
>

### Create a Systemd Unit File
22. To run Odoo as a service, there is a need to create a service unit file in the ```/etc/systemd/system/``` directory. For this, open the text editor:

```sh
$ sudo nano /etc/systemd/system/odoo12.service
```
23. Paste the following to the editor:
```sh
[Unit]
Description=Odoo12
Requires=postgresql.service
After=network.target postgresql.service

[Service]
Type=simple
SyslogIdentifier=odoo12
PermissionsStartOnly=true
User=odoo12
Group=odoo12
ExecStart=/opt/odoo12/odoo-venv/bin/python3 /opt/odoo12/odoo/odoo-bin -c /etc/odoo12.conf
StandardOutput=journal+console

[Install]
WantedBy=multi-user.target
```
24. Notify systemd that a new unit file exists and start the Odoo service by:
```sh
$ sudo systemctl daemon-reload
$ sudo systemctl start odoo12
```
### Check the odoo service status and enable service at start
```sh
$ sudo systemctl status odoo12
```
25. If you wish to enable Odoo service to be automatically started at boot time:
```sh
$ sudo systemctl enable odoo12
```
### Deployment
26. Check out http://localhost:8069/ on your favorite browser.
