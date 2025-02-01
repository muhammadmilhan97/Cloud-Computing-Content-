# Creating an Azure Virtual Machine (Ubuntu) and Connecting via SSH

This guide provides a step-by-step process for creating an Azure Virtual Machine (VM) running Ubuntu and connecting to it using SSH from PowerShell.

## Prerequisites
- An active [Microsoft Azure](https://portal.azure.com/) account
- Basic understanding of the Azure portal
- PowerShell installed on your local machine (available by default on Windows)

## Step 1: Sign in to Azure Portal
1. Open your browser and go to [Azure Portal](https://portal.azure.com/).
2. Log in with your Microsoft account.

## Step 2: Create an Ubuntu Virtual Machine
1. In the Azure Portal, search for **"Virtual Machines"** in the search bar.
2. Click **"Create"** → **"Azure Virtual Machine"**.
3. Configure the following settings:
   - **Subscription**: Select your active subscription.
   - **Resource Group**: Click **"Create new"** or select an existing one.
   - **Virtual Machine Name**: Provide a unique name.
   - **Region**: Choose the appropriate data center location.
   - **Image**: Select **Ubuntu (e.g., Ubuntu 22.04 LTS)**.
   - **Size**: Choose an appropriate VM size.
   - **Administrator Account**: Select **SSH Public Key** authentication.
   - **Inbound Port Rules**: Allow **SSH (22)**.
4. Click **Next** for additional configurations (or keep defaults).
5. Click **"Review + Create"** → **"Create"**.

## Step 3: Retrieve SSH Connection Details
1. Once the deployment is complete, navigate to **Virtual Machines** in the Azure Portal.
2. Select your newly created Ubuntu VM.
3. Copy the **Public IP Address** from the overview page.

## Step 4: Connect to Ubuntu VM via SSH using PowerShell
1. Open **PowerShell** on your local machine.
2. Navigate to the **Downloads** folder where the `.pem` file is saved:
   ```powershell
   cd Downloads
   ```
3. Use the following command to connect (replace `filename.pem` with your actual key file name, `vm-user` with your actual username, and `<your-vm-ip>` with the copied IP address):
   ```powershell
   ssh -i filename.pem vm-user@<your-vm-ip>
   ```
4. If prompted, type **yes** to accept the SSH connection.
5. You are now connected to your Azure Ubuntu VM.

## Step 5: Configure Firewall for SSH (If Needed)
If you encounter connection issues:
1. Go to **Azure Portal** → Navigate to your VM.
2. Click **Networking**.
3. Ensure **SSH (TCP/22)** is allowed in **Inbound Rules**.
4. Restart the VM if necessary.

## Conclusion
You have successfully created an Azure Virtual Machine running Ubuntu and connected to it using SSH via PowerShell. You can now install software, configure settings, and manage your VM from the command line.

Enjoy your Azure Ubuntu VM!
