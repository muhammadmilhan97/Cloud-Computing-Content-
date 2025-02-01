# Creating an Azure Virtual Machine (Windows) and Connecting via RDP

This guide will walk you through the process of creating an Azure Virtual Machine (VM) running Windows OS and connecting to it using Remote Desktop Protocol (RDP).

## Prerequisites
- An active [Microsoft Azure](https://portal.azure.com/) account
- Basic understanding of Azure portal
- Remote Desktop Client (pre-installed on Windows; available for macOS & Linux)

## Step 1: Sign in to Azure Portal
1. Go to [Azure Portal](https://portal.azure.com/).
2. Sign in with your Microsoft account.

## Step 2: Create a Virtual Machine
1. In the Azure Portal, search for **"Virtual Machines"** in the search bar.
2. Click **"Create"** → **"Azure Virtual Machine"**.
3. Configure the following settings:
   - **Subscription**: Select your active subscription.
   - **Resource Group**: Click **"Create new"** or select an existing one.
   - **Virtual Machine Name**: Provide a unique name.
   - **Region**: Choose the data center location.
   - **Image**: Select a Windows OS (e.g., Windows Server 2022, Windows 10, etc.).
   - **Size**: Choose an appropriate VM size based on your requirements.
   - **Administrator Account**: Set up a username and password.
   - **Inbound Port Rules**: Allow **RDP (3389)**.
4. Click **Next** for additional configurations (or keep defaults).
5. Click **"Review + Create"** → **"Create"**.

## Step 3: Get the RDP Connection Details
1. Once deployment is complete, go to the **Virtual Machines** section.
2. Select your newly created VM.
3. Click on the **Connect** button at the top.
4. Choose **RDP**.
5. Click **Download RDP File**.

## Step 4: Connect to the VM using RDP
1. Open the downloaded `.rdp` file.
2. Click **Connect**.
3. Enter the **Administrator username and password** set during VM creation.
4. Click **OK** and **Accept** any security warnings.
5. You should now be connected to your Azure Virtual Machine.

## Step 5: Configure Firewall for RDP (If Needed)
If you face issues connecting via RDP:
1. Open the **Azure Portal** → Go to your VM.
2. Navigate to **Networking**.
3. Ensure **RDP (TCP/3389)** is allowed under **Inbound Rules**.
4. Restart the VM if necessary.

## Conclusion
You have successfully created an Azure Virtual Machine running Windows and connected to it using RDP. You can now install software, manage resources, or configure additional security settings as needed.

Enjoy your Azure VM!
