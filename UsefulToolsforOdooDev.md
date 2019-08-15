## Installing PyCharm Community Edition
For Odoo development, Pycharm IDE could be exploited. The community edition of Pycharm can be installed using the command below:
```sh
$ sudo snap install pycharm-community --classic
```

## Installing DBeaver Community

DBeaver is a useful database tool for generating the entity relationships diagrammatically for selected/all tables available in a database. The following commands can be used to install/upgrade DBeaver on Debian Linux systems:

```sh
$ sudo add-apt-repository ppa:serge-rider/dbeaver-ce
$ sudo apt-get update
$ sudo apt-get install dbeaver-ce 
```

## Installing pgAdmin4 Tool
pgAdmin4 packages are available under PostgreSQL official apt repository. To install pgAdmin4, execute the command below:

```sh
$ sudo apt-get install pgadmin4 pgadmin4-apache2
```
The next step involves setting up pgAdmin4 admin login. For this, enter an e-mail address on the prompt to use as admin login user for pgAdmin4 web interface and then press OK.

![](/images/pgAmin_prompt1.png)

Then set the pgAdmin4 password on the next prompt:

![](/images/prompt2.png)

Now to access pgAdmin4 on your browser, use the IP address of the server or domain name followed by /pgadmin4: 

```sh
http://localhost/pgadmin4/
```
Login with the admin login name and password set during the installation process as illustrated below:

![](/images/pgadmi4_login_page.png)

Having successfully logged-in, now add a new server as shown below, supplying the pop-up with the details:

![](/images/pgadmin_new_server.png)

Switching to the connection tab, enter the hostname or IP address of your PostgreSQL instance, with the username and password for PostgreSQL. Close the window after saving the respective settings.

![](/images/create_server_conn.png)

Now the instance created would be seen on the left sidebar, ready for management.
