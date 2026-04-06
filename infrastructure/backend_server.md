# Follow these installation steps on the AMI EC2 instance:

## Install PHP, Apache, and MySQL client:
https://docs.aws.amazon.com/linux/al2023/ug/ec2-lamp-amazon-linux-2023.html

## Install git:
```
sudo dnf install git -y
```

## Add the following EC2 instance userdata to the Launch Template:

```
#!/bin/bash

# aggiorna sistema
yum update -y

# installa web server + php + sql + git
yum install -y httpd php php-mysqlnd mariadb105-server git

# avvia apache
systemctl start httpd
systemctl enable httpd

# entra nella directory web
cd /var/www/html

# scarica codice
sudo git clone https://github.com/AxelNyagom-cloud/aws-three-tier-architecture.git

# crea cartella API
sudo mkdir -p /var/www/html/api

# copia solo backend API
sudo cp -r /var/www/html/aws-three-tier-architecture/backend/api/* /var/www/html/api/

# rimuove repo inutile
sudo rm -rf /var/www/html/aws-three-tier-architecture

# CONFIG DB (IMPORTANTE)
sudo sed -i 's/update-me-host/YOUR-RDS-ENDPOINT/g' /var/www/html/api/db_connection.php
sudo sed -i 's/update-me-username/admin/g' /var/www/html/api/db_connection.php
sudo sed -i 's/update-me-password/YOUR_PASSWORD/g' /var/www/html/api/db_connection.php

# permessi corretti
sudo chown -R apache:apache /var/www/html
sudo chmod -R 755 /var/www/html

# restart apache
systemctl restart httpd

```
