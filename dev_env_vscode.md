


```sh
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Odoo 16",
            "type": "python",
            "request": "launch",
            "args": [
                "--config",
                "${workspaceFolder}/odoo/debian/odoo.conf",
                //"-d","test1"
            ],
            "program": "${workspaceFolder}/odoo/odoo-bin",
            "python": "${workspaceFolder}/odoo16-venv/bin/python3",
            "env": {
                "PYTHONPATH": "${workspaceFolder}/odoo/"
            },
            "console": "integratedTerminal",
            "justMyCode": true
            
        }

    ]
}
```

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