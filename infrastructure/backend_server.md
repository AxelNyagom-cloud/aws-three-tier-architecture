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

yum update -y

yum install -y httpd php php-mysqlnd php-pdo php-pdo_mysql mariadb105-server git

systemctl start httpd
systemctl enable httpd

cd /var/www/html

sudo git clone https://github.com/AxelNyagom-cloud/aws-three-tier-architecture.git

sudo mkdir -p /var/www/html/api

sudo cp -r aws-three-tier-architecture/backend/api/* /var/www/html/api/

sudo rm -rf aws-three-tier-architecture

# CONFIG DB
sudo sed -i 's/update-me-host/YOUR-RDS-ENDPOINT/g' /var/www/html/api/db_connection.php
sudo sed -i 's/update-me-username/admin/g' /var/www/html/api/db_connection.php
sudo sed -i 's/update-me-password/YOUR_PASSWORD/g' /var/www/html/api/db_connection.php

# PERMESSI
sudo usermod -a -G apache ec2-user
sudo chown -R ec2-user:apache /var/www
sudo chmod 2775 /var/www
sudo find /var/www -type d -exec chmod 2775 {} \;
sudo find /var/www -type f -exec chmod 0664 {} \;

systemctl restart httpd

```
