# Hosting a Website Using Nginx on Ubuntu: Step-by-Step Guide

This guide walks you through setting up a website using Nginx on an Ubuntu server, including cloning a repository and configuring Nginx correctly.

## Prerequisites
- A server running Ubuntu (20.04 or later recommended)
- A non-root user with sudo privileges
- Domain name (optional but recommended)
- Git installed on the server

## Step 1: Update System Packages
Before installing Nginx, update the package lists and upgrade existing packages:
```bash
sudo apt update && sudo apt upgrade -y
```

## Step 2: Install Nginx
Install Nginx using the following command:
```bash
sudo apt install nginx -y
```

After installation, verify that Nginx is running:
```bash
systemctl status nginx
```
If it's not running, start it manually:
```bash
sudo systemctl start nginx
```
Enable it to start at boot:
```bash
sudo systemctl enable nginx
```

## Step 3: Adjust Firewall Rules
Allow HTTP and HTTPS traffic:
```bash
sudo ufw allow 'Nginx Full'
```
Check the firewall status:
```bash
sudo ufw status
```

## Step 4: Clone the Website Repository
Navigate to the web root directory:
```bash
cd /var/www/
```
Clone your repository (replace `<repository-url>` with your actual GitHub/GitLab repository URL):
```bash
sudo git clone <repository-url> mywebsite
```
Set the correct permissions:
```bash
sudo chown -R www-data:www-data /var/www/mywebsite
sudo chmod -R 755 /var/www/mywebsite
```

## Step 5: Configure Nginx for Your Website
Create a new Nginx configuration file:
```bash
sudo nano /etc/nginx/sites-available/mywebsite
```
Add the following configuration (replace `yourdomain.com` with your actual domain or server IP):
```nginx
server {
    listen 80;
    server_name yourdomain.com www.yourdomain.com;
    root /var/www/mywebsite;
    index index.html index.htm index.php;
    location / {
        try_files $uri $uri/ =404;
    }
}
```
Save and exit the file.

## Step 6: Enable the Site Configuration
Create a symbolic link to enable the configuration:
```bash
sudo ln -s /etc/nginx/sites-available/mywebsite /etc/nginx/sites-enabled/
```
Test the Nginx configuration for syntax errors:
```bash
sudo nginx -t
```
Restart Nginx to apply changes:
```bash
sudo systemctl restart nginx
```

## Step 7: Verify the Setup
Open a web browser and navigate to `http://yourdomain.com`. You should see your website live.

## Step 8: Enable Automatic Updates (Optional)
To keep Nginx and other packages updated:
```bash
sudo apt install unattended-upgrades -y
```
Enable automatic updates:
```bash
sudo dpkg-reconfigure unattended-upgrades
```

## Conclusion
Your website is now hosted on an Ubuntu server using Nginx. You have set up Nginx, cloned a repository, and configured the server. To update your website in the future, navigate to `/var/www/mywebsite` and pull the latest changes:
```bash
cd /var/www/mywebsite
sudo git pull origin main
```

Enjoy your self-hosted website!
