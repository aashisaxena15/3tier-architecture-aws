# Follow these installation steps on the AMI EC2 instance:

## Install Nginx server and git
```
sudo dnf install nginx -y
sudo dnf install git -y
```
Replace the server block in /etc/nginx/nginx.conf with the content from the nginx_config file.

## Add the following EC2 instance userdata to the Launch Template:

```
#!/bin/bash

# Update package list and install nginx & git
sudo apt-get update -y
sudo apt-get install -y nginx git

# Replace placeholder in nginx config with actual backend ALB URL
sudo sed -i 's/update-me/internal-backend-alb-81789294.ap-south-1.elb.amazonaws.com/g' /etc/nginx/nginx.conf

# Start and enable nginx service
sudo systemctl start nginx
sudo systemctl enable nginx

# Navigate to nginx's web root
cd /usr/share/nginx/html

# Clone your frontend code from GitHub
sudo git clone https://github.com/aashisaxena15/3tier-architecture-aws.git

# Move frontend files to the nginx root
sudo mv 3tier-architecture-aws/frontend/* .

# Clean up the cloned repo
sudo rm -rf 3tier-architecture-aws

```


