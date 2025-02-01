Running a Windows-based image in a Docker container using PowerShell involves several steps. Here’s a step-by-step guide along with explanations:

-----
**Prerequisites**

1. **Install Docker**: Ensure that Docker Desktop is installed on your Windows machine. You can download it from [Docker's official website](https://www.docker.com/products/docker-desktop/).
1. **Enable Windows Containers**: By default, Docker Desktop runs in **Linux container mode**. You need to switch to **Windows containers** mode.
   1. Right-click the Docker icon in the system tray.
   1. Click **"Switch to Windows containers"**.
-----
**Step-by-Step Guide**

**Step 1: Verify Docker Installation**

Open **PowerShell** as Administrator and check if Docker is installed and running:

docker version

This will display details about the Docker client and server.

To check if Docker is running:

docker info

If Docker is not running, start it from the system tray or run:

Start-Service docker

-----
**Step 2: Pull a Windows Image**

Docker pulls container images from **Docker Hub**. To pull a Windows-based image, use:

docker pull mcr.microsoft.com/windows/servercore:ltsc2022

🔹 This pulls the **Windows Server Core** image (LTSC 2022 version).
🔹 You can also pull other Windows images like **Nano Server**:

docker pull mcr.microsoft.com/windows/nanoserver:ltsc2022

-----
**Step 3: List Available Images**

After pulling the image, verify it using:

docker images

This shows all downloaded images, including their **repository name, tag, image ID, and size**.

-----
**Step 4: Run the Windows Container**

To run a container from the Windows image, use:

docker run -it mcr.microsoft.com/windows/servercore:ltsc2022 cmd

🔹 -it: Runs the container in **interactive mode** (allows user input).
🔹 cmd: Starts a command prompt inside the container.

Alternatively, to run PowerShell inside the container:

docker run -it mcr.microsoft.com/windows/servercore:ltsc2022 powershell

-----
**Step 5: Check Running Containers**

In another PowerShell window, check running containers:

docker ps

This displays active containers along with their **container ID, image name, status, and ports**.

To see all containers (including stopped ones):

docker ps -a

-----
**Step 6: Access an Existing Running Container**

If you exit the container but need to access it again, first get its **Container ID**:

docker ps -a

Then, use the exec command to open a command prompt inside the running container:

docker exec -it <container\_id> cmd

Or to start PowerShell inside the container:

docker exec -it <container\_id> powershell

-----
**Step 7: Stop and Remove Containers**

To stop a running container:

docker stop <container\_id>

To remove a container:

docker rm <container\_id>

To remove all stopped containers:

docker container prune

-----
**Step 8: Remove Docker Images**

To delete an image:

docker rmi mcr.microsoft.com/windows/servercore:ltsc2022

To remove all unused images:

docker image prune

-----
**Optional: Running a Windows Container in Detached Mode**

If you want to run a Windows container in the background (detached mode), use:

docker run -d --name mycontainer mcr.microsoft.com/windows/servercore:ltsc2022

🔹 -d: Runs the container in **detached mode** (background).
🔹 --name mycontainer: Assigns a name to the container.

To check logs of a running container:

docker logs mycontainer

To stop the container:

docker stop mycontainer

-----
**Final Notes**

- Windows containers **cannot** run **Linux-based images**, so ensure you are in **Windows container mode**.
- The Windows container images are large (several GBs), so downloading them might take time.
- For GUI-based Windows applications, you might need to use **Windows Server Core with GUI support**.
-----
## 🔹 Alternative Ways to Install WinGet in the Container
Since **WinGet is not officially supported in Windows Server Core**, try these workarounds:
### **✅ Option 1: Install Chocolatey Instead of WinGet**
If you only need a package manager, **Chocolatey** is a great alternative. Run this inside the container:

Set-ExecutionPolicy Bypass -Scope Process -Force

[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072

Invoke-Expression ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))

Then, install Git (or any package) using:

choco install git -y

**1. Set Up Git**

- Ensure Git is installed on your local machine. 
  - To check, run: 
  - git --version

    If not installed, download it from [git-scm.com](https://git-scm.com/).

-----
**2. Navigate to Your Website Directory**

- Open your terminal and navigate to the folder containing your website files: 
- cd /path/to/your/website
-----
**3. Initialize a Local Git Repository**

- If your project isn’t already a Git repository, initialize it: 
- git init
-----
**4. Add Remote Repository**

- Add the remote Git repository URL: 
- git remote add origin <REMOTE\_URL>

  Replace <REMOTE\_URL> with the URL of your Git repository (e.g., on GitHub, GitLab, or Bitbucket).

-----
**5. Add Files to the Repository**

- Stage all the website files: 
- git add .
-----
**6. Commit the Files**

- Commit the staged files with a descriptive message: 
- git commit -m "Initial commit: Add website files"
-----
**7. Push Files to Remote Repository**

- Push the committed files to the remote repository: 
- git push -u origin main

  Replace main with your branch name if it's different (e.g., master).

-----
**8. Set Up Git Branch (Optional)**

- If the branch doesn't exist yet, you may need to create it: 
- git branch -M main
- git push -u origin main
-----
**9. Verify**

- Visit your Git hosting platform to ensure the files are uploaded.
-----

