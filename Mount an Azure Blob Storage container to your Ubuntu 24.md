Mount an Azure Blob Storage container to your Ubuntu 24.04 VM using Blobfuse2. Below is a **step-by-step** guide with explanations to ensure a smooth setup.

-----
**Step 1: Install Blobfuse2**

Blobfuse2 is required to mount Azure Blob Storage as a filesystem. <https://www.aloneguid.uk/posts/2024/05/blobfuse2-ubuntu2404/> Run the following commands:

\# Download the Microsoft package repository

sudo wget https://packages.microsoft.com/config/ubuntu/24.04/packages-microsoft-prod.deb

\# Install the Microsoft repository package

sudo dpkg -i packages-microsoft-prod.deb

\# Update the package list

sudo apt-get update

\# Install required dependencies

sudo apt-get install libfuse3-dev fuse3 -y

\# Install Blobfuse2

sudo apt-get install blobfuse2 -y

**Verification:** Check if Blobfuse2 is installed by running:

blobfuse2 --version

-----
**Step 2: Configure Blobfuse2**

Create a directory to store the Blobfuse configuration file:

mkdir -p ~/.blobfuse2

cd ~/.blobfuse2

Create a configuration file named blobfuse.yml using a text editor:

nano ~/.blobfuse2/blobfuse.yml

**Add the following configuration:**

allow-other: true

logging:

`  `type: syslog

`  `level: log\_debug

components:

`  `- libfuse

`  `- file\_cache

`  `- attr\_cache

`  `- azstorage

libfuse:

`  `attribute-expiration-sec: 120

`  `entry-expiration-sec: 120

`  `negative-entry-expiration-sec: 240

file\_cache:

`  `path: /mnt/blobfuse-cache

`  `timeout-sec: 120

`  `max-size-mb: 4096

attr\_cache:

`  `timeout-sec: 7200

azstorage:

`  `type: block

`  `account-name: <your-storage-account-name>

`  `account-key: <your-storage-account-key>

`  `endpoint: https://<your-storage-account-name>.blob.core.windows.net

`  `mode: key

`  `container: <your-container-name>

**Replace the placeholders** (<your-storage-account-name>, <your-storage-account-key>, <your-container-name>) with actual values from your Azure Storage account.

Save the file (CTRL + X, then Y, then ENTER).

-----
**Step 3: Create a Local Cache Directory**

Blobfuse2 requires a local cache directory for temporary storage:

sudo mkdir -p /mnt/blobfuse-cache

sudo chmod 777 /mnt/blobfuse-cache

-----
**Step 4: Create a Mount Point and Mount the Storage**

Create a mount directory:

sudo mkdir -p /mnt/blobstorage

Mount the Blob Storage:

sudo blobfuse2 mount /mnt/blobstorage --config-file=/home/<your-username>/.blobfuse2/blobfuse.yml

**Verify the mount:** Run:

ls /mnt/blobstorage

You should see the contents of your Azure Blob container.

-----
**Step 5: Persist the Mount After Reboot**

To make the mount persistent, add it to /etc/fstab:

1. Open /etc/fstab in a text editor:
1. sudo nano /etc/fstab
1. Add the following line at the end of the file:
1. blobfuse2 /mnt/blobstorage fuse3 defaults,\_netdev,--config-file=/home/<your-username>/.blobfuse2/blobfuse.yml,allow\_other 0 0
1. Save and exit (CTRL + X, then Y, then ENTER).
1. **Test fstab entry** (to ensure no errors):
1. sudo mount -a
1. **Reboot the VM** and check if the mount persists:
1. sudo reboot
-----
**Troubleshooting Errors**

If you encounter errors, try:

1. **Checking logs**:
1. journalctl -xe | grep blobfuse

   or

   cat /var/log/syslog | grep blobfuse

1. **Running Blobfuse in Debug Mode**:
1. sudo blobfuse2 mount /mnt/blobstorage --config-file=/home/<your-username>/.blobfuse2/blobfuse.yml --foreground --log-level=log\_debug
1. **Ensuring permissions are correct**:
1. sudo chmod 777 /mnt/blobfuse-cache
1. sudo chmod 777 /mnt/blobstorage
1. **Checking if FUSE3 is running**:
1. lsmod | grep fuse
1. **Reinstalling Blobfuse2**:
1. sudo apt-get remove --purge blobfuse2 -y
1. sudo apt-get install blobfuse2 -y
-----

Yes, once you've successfully mounted your **Azure Blob Storage** container to your Ubuntu **VM** using **Blobfuse2**, you can create, modify, and delete files inside the mount directory (/mnt/blobstorage). These changes will **directly reflect in your Azure Storage Blob Container**.

-----
### **Testing the Mount with File Creation**
After mounting your storage to /mnt/blobstorage, test it by creating a file:

cd /mnt/blobstorage

echo "Hello from Azure VM" > testfile.txt

ls -l

If the mount is working correctly, the testfile.txt should now be visible in your **Azure Storage Blob Container**.

-----
### **Verify in Azure Portal**
1. Go to **Azure Portal** → Your **Storage Account**.
1. Navigate to the **Blob Container** (the one you configured in blobfuse.yml).
1. Check if testfile.txt is visible.
-----
### **Uploading & Downloading Files**
#### *Upload a Local File to Azure Blob Storage*
cp /home/<your-username>/localfile.txt /mnt/blobstorage/

After this command, localfile.txt will be in Azure Blob Storage.
#### *Download a File from Azure Blob Storage*
cp /mnt/blobstorage/testfile.txt /home/<your-username>/downloaded\_file.txt

This copies testfile.txt from Blob Storage to your local machine.

-----
### **Persistent Storage Behavior**
- Any **files/folders created, deleted, or modified** inside /mnt/blobstorage/ will directly reflect in the Azure **Blob Container**.
- However, **Blobfuse2 does not support** **append blob updates** or **block blob partial modifications**—it treats blobs as files.
-----
### **Troubleshooting If File Does Not Appear**
If files do not appear in **Azure Portal**:

1. **Check if mount is active:**
1. df -h | grep blob

   If you don’t see your mount, try remounting.

1. **Restart the mount:**
1. sudo blobfuse2 mount /mnt/blobstorage --config-file=/home/<your-username>/.blobfuse2/blobfuse.yml
1. **Ensure correct permissions:**
1. sudo chmod -R 777 /mnt/blobstorage
1. **List files to confirm they exist in the mount:**
1. ls -lah /mnt/blobstorage

Let me know if you run into any issues! 🚀

