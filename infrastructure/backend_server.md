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

# Update system packages and install Apache & Git
sudo yum update -y
sudo yum install -y httpd git

# Start and enable Apache service
sudo systemctl start httpd
sudo systemctl enable httpd

# Navigate to Apache web root
cd /var/www/html

# Clone backend code from your GitHub repository
sudo git clone https://github.com/aashisaxena15/3tier-architecture-aws.git

# Create api directory and move backend files
sudo mkdir api
sudo mv 3tier-architecture-aws/backend/api/* api/

# Clean up the cloned repository
sudo rm -rf 3tier-architecture-aws

# Replace database credentials in db_connection.php
sudo sed -i 's/update-me-host/demo-database.cb0qe2wsafpo.ap-south-1.rds.amazonaws.com/g' /var/www/html/api/db_connection.php
sudo sed -i 's/update-me-username/admin/g' /var/www/html/api/db_connection.php
sudo sed -i 's/update-me-password/aashi123/g' /var/www/html/api/db_connection.php
```

