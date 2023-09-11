## Documentation for Setting up Odoo 

This tutorial covers the steps needed for installing and configuring Odoo 16 Community version from Git source on a Python virtual environment on an _Ubuntu 20.04_ system. 

### Updating the System to Latest Packages
1. Open up the Linux terminal as a sudo user and execute the commands below in a step by step manner:

```sh
sudo apt-get update
sudo apt-get upgrade
```
### Install Python 3 and its Dependencies
2. Install the required python packages for Odoo:

#### Install pip3:

```sh
sudo apt-get install -y python3-pip
```
#### Then install Packages and libraries:

```sh
sudo apt-get install python-dev python3-dev libxml2-dev libxslt1-dev zlib1g-dev libsasl2-dev libldap2-dev build-essential libssl-dev libffi-dev libmysqlclient-dev libjpeg-dev libpq-dev libjpeg8-dev liblcms2-dev libblas-dev libatlas-base-dev
```

#### web dependencies are also needed to be installed:


```sh
sudo apt-get install -y npm
sudo ln -s /usr/bin/nodejs /usr/bin/node
sudo npm install -g less less-plugin-clean-css
sudo apt-get install -y node-less
```

### Installing Wkhtmlopdf tool

3. Odoo supports printing reports as PDF files. Wkhtmltopdf helps to generate PDF reports from HTML data format. Moreover, the Qweb template reports are converted to HTML format by the report engine and Wkhtmltopdf will produce the PDF report:

```sh
sudo wget https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6-1/wkhtmltox_0.12.6-1.focal_amd64.deb
sudo apt install -f ./wkhtmltox_0.12.6-1.focal_amd64.deb
```

### Enabling PostgreSQL Apt Repository, Installing and Configuring PostgreSQL 
4. Enable PostgreSQL Apt Repository by importing the GPG key for PostgreSQL packages:

```sh
sudo apt-get install postgresql
```

5. Create a Database User Role for Handling Odoo Databases
Next, a password for the distinctive user should be defined, which is needed later in the conf file:

```sh
sudo su - postgres
```

Next, a password for the distinctive user should be defined, which is needed later in the conf file:
```sh
createuser --createdb --username postgres --no-createrole --no-superuser --pwprompt odoo16
```
The following command ensures that the user has superuser access rights:
```sh
psql
ALTER USER odoo16 WITH SUPERUSER;
```
Exit from psql and Postgres user:
```sh
\q
exit
```
### Installing git 
6. To clone source code from git have to install git, follow the below commands:

```sh
sudo apt-get install git
```

Configure your [git](https://www.odoo.com/documentation/16.0/contributing/development/git_guidelines.html)

```sh
git config --global user.name "value"
git config --global user.email "value"
```

### Creating a System User for Odoo
7. Create a new system user named ```odoo16``` with a home directory ```/opt/odoo16``` by:

```sh
sudo useradd -m -d /opt/odoo16 -U -r -s /bin/bash odoo16
```

### Installing and Configuring Odoo
8. Switch to ```odoo16``` user:

```sh
sudo su - odoo16
```

9. Clone Odoo 16 community version from Git source:

```sh
git clone https://www.github.com/odoo/odoo --depth 1 --branch 16.0 /opt/odoo16/odoo
```

10. Creating a new Python Virtual Environment:

```sh
cd /opt/odoo16
python3 -m venv odoo16-venv
```

11. Activate the virtual environment:

```sh
source odoo16-venv/bin/activate
```

12. Install all of the required Python modules with pip3:

```sh
pip3 install wheel
pip3 install -r odoo/requirements.txt
```

13. Deactivate the virtual environment:

```sh
deactivate
```

14. Create a new directory for custom addons:

```sh
mkdir /opt/odoo16/odoo-custom-addons
```

15. Switch back to sudo user:

```sh
exit
```

16. Create a configuration file:

```sh
sudo cp /opt/odoo16/odoo/debian/odoo.conf /etc/odoo16.conf
```

17. Open the configuration file:

```sh
sudo nano /etc/odoo16.conf
```

18. Edit the configuration file as:

```sh
[options]
; This is the password that allows database operations:
;admin_passwd = enter-your-password
db_host = False
db_port = False
db_user = odoo16
db_password = False
addons_path = /opt/odoo16/odoo/addons,/opt/odoo16/odoo-custom-addons
```
>
>Make sure to change the ```enter-your-password``` indicated above
>db_password change `Flase` to password you defined while creating postrgress user 
>

### Launch Odoo

19. Run Odoo as a [service](odooServiceSetup.md) 
20. Run Odoo with development enviroment: [VSCODE](dev_env_vscode.md) [PYCHARM](dev_env_pycharm.md)