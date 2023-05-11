### Create a Systemd Unit File
1. To run Odoo as a service, there is a need to create a service unit file in the ```/etc/systemd/system/``` directory. For this, open the text editor:

```sh
sudo nano /etc/systemd/system/odoo16.service
```

2. Paste the following to the editor:

[Unit]
Description=Odoo16
Requires=postgresql.service
After=network.target postgresql.service

[Service]
Type=simple
SyslogIdentifier=odoo16
PermissionsStartOnly=true
User=odoo16
Group=odoo16
ExecStart=/opt/odoo16/odoo-venv/bin/python3 /opt/odoo16/odoo/odoo-bin -c /etc/odoo16.conf
StandardOutput=journal+console

[Install]
WantedBy=multi-user.target
```
3. Notify systemd that a new unit file exists and start the Odoo service by:
```sh
sudo systemctl daemon-reload
sudo systemctl start odoo16
```
### Check the odoo service status and enable service at start
```sh
sudo systemctl status odoo16
```
4. If you wish to enable Odoo service to be automatically started at boot time:
```sh
sudo systemctl enable odoo16
```
### Deployment
5. Check out http://localhost:8069/ on your favorite browser.