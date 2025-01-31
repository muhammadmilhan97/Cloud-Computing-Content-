# Creating an Instance of Ubuntu on AWS and Connecting via SSH in PowerShell

## Introduction
Launching an Ubuntu instance on Amazon Web Services (AWS) and connecting to it using SSH in PowerShell enables users to remotely manage their cloud-based Linux server. This guide provides a step-by-step approach to creating an Ubuntu EC2 instance and connecting via SSH using PowerShell.

## Prerequisites
Before starting, ensure you have the following:

- An AWS account
- Basic knowledge of AWS EC2 services
- PowerShell installed on your local machine (Windows comes with it by default)
- OpenSSH client installed on Windows (enabled by default in newer versions)

## Step 1: Launch an Ubuntu EC2 Instance

### 1. Log in to AWS Console
- Navigate to [AWS Console](https://aws.amazon.com/console/).
- Click on **EC2** under the "Services" menu.

### 2. Launch an Instance
- Click on the **Launch Instance** button.

- Choose **Ubuntu Server** as the Amazon Machine Image (AMI).

- Select an instance type (e.g., **t2.micro** for free-tier users).

- Click **Next: Configure Instance Details** and set required configurations.

### 3. Configure Storage and Security Group
- Set the required storage size (e.g., 10GB for Ubuntu default settings).
- Configure the security group:

  - Allow **SSH (TCP 22)** under inbound rules.

  - Allow your public IP or **0.0.0.0/0** (not recommended for security reasons).

- Click **Review and Launch**.

### 4. Launch and Download Key Pair
- Create a new key pair or use an existing one.

- Download the **.pem** file and store it securely.

- Click **Launch Instance** and wait for it to initialize.

## Step 2: Connect to the Ubuntu Instance via SSH in PowerShell

### 1. Open PowerShell

- Press `Win + X` and select **PowerShell** or **Windows Terminal**.


### 2. Navigate to the Directory Containing the `.pem` File
- Use the `cd` command to change the directory to where your **.pem** file is stored.

  ```powershell
  cd C:\path\to\your\pem\file
  ```

### 3. Set Correct Permissions for the Key File
- Modify the permissions to make the key secure.

  ```powershell
  icacls "your-key.pem" /inheritance:r
  icacls "your-key.pem" /grant:r "%username%:R"
  ```

### 4. Connect to the Ubuntu Instance via SSH
- Find the **Public IPv4 address** of your instance from the EC2 Dashboard.

- Use the following command to establish an SSH connection:

  ```powershell
  ssh -i "your-key.pem" ubuntu@your-instance-ip
  ```

### 5. Accept the Connection

- The first time you connect, you may see a fingerprint verification prompt. Type **yes** and press Enter.


## Step 3: Secure and Manage Your Instance

### 1. Update the System

Run the following commands after logging in:

  ```sh
  sudo apt update && sudo apt upgrade -y
  ```

### 2. Create a New User (Optional but Recommended)

  ```sh
  sudo adduser newuser
  sudo usermod -aG sudo newuser
  ```

### 3. Enable Firewall

  ```sh
  sudo ufw allow OpenSSH
  sudo ufw enable
  ```

## Conclusion

By following these steps, you can successfully create an Ubuntu EC2 instance on AWS and connect to it using SSH in PowerShell. Ensure that security best practices are followed to protect your instance from unauthorized access.
