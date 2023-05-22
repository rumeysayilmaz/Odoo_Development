

change odoo.conf settings `/odoo/debian/odoo.conf`

```sh
[options]
; Is This The Password That Allows Database Operations:
;admin_passwd = admin
db_host = localhost
db_port = 5432
db_user = odoo16
db_password = "enter-your-db-password"
addons_path = /opt/odoo16/odoo/addons,/opt/odoo16/odoo-custom-addons
```