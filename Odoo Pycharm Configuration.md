## Pycharm Configuration for Odoo
If the odoo12 service is running then as a first step stop the odoo12 service:
```sh
$ sudo systemctl stop odoo12
```
On linux terminal, grant read write access to the ```/opt/odoo12``` directory:
```sh
sudo chmod -R 777 /opt/odoo12
```
On linux terminal, grant read write access to the ```/snap/pycharm-community``` directory:
```sh
sudo chmod -R 777 /snap/pycharm-community
```
Run Pycharm, from the view tab enable the toolbar.

Add a configuration for the project as follows:
![](/images/pycharm_config.png)

Run the Project from Pycharm IDE environment. An error will appear on the screen saying that the username (owner name of the computer) doesn't exist hence for this purpose we need to create a user for the owner name of computer. In our case the name was administrator:
```sh
sudo su - postgres -c "createuser -s administrator"
```
Run the project on Pycharm. The following odoo start screen shall appear when you check out http://localhost:8069 on your browser:
![](/images/odoo-12.jpg)
>
>**IMPORTANT**: This page asks for re-creation of a postgreSQL database for the odoo. If you don't wish to re-create a seperate database (use the database created during  odoo installation) then in that case you have to alter the owner of the database created earlier. For this, follow the instructions below:
>
Terminate pycharm pogram. From the Linux terminal, open the odoo12 configuration file:
```sh
$ sudo nano /etc/odoo12.conf
```
Edit the file by changing ```db_user = administrator``` and save the file.

Then, open the odoo12 service file:
```sh
$ sudo nano /etc/systemd/system/odoo12.service
```
Edit the file by changing ```User = administrator``` and save the file.

Notify systemd that a new unit file exists and start the Odoo service:
```sh
$ sudo systemctl daemon-reload
$ sudo systemctl start odoo12
```
If you wish to enable Odoo service to be automatically started at boot time:
```sh
$ sudo systemctl enable odoo12
```
> Note that if you wish to run your odoo code developed in Pycharm, then the odoo12 service mentioned above needs to be disabled.  
