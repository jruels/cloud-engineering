### **Using Chocolatey to manage applications**

---

### **Step 1: Install Chocolatey**

1. **Open PowerShell in Administrator Mode**:
   - Press the **Windows key**, type **PowerShell**, right-click **Windows PowerShell**, and select **Run as administrator**.

2. **Run the Chocolatey Installation Command**:
   - In the PowerShell window, run the following command to install Chocolatey:

   ```powershell
   Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
   ```

3. **Verify Chocolatey Installation**:
   - Once the installation is complete, check that Chocolatey was installed correctly by typing the following command:

   ```powershell
   choco --version
   ```

   - The output should display the current version of Chocolatey.

---

### **Step 2: Install Software Using Chocolatey**

1. **Install Git Using Chocolatey**:
   - Now that Chocolatey is installed, let’s install **Git** as an example software package. Run the following command:

   ```powershell
   choco install git -y
   ```

   - The `-y` flag automatically agrees to all prompts during installation.

2. **Verify Git Installation**:
   - To ensure Git was successfully installed, check its version by running:

   ```powershell
   git --version
   ```

   - You should see the installed version of Git in the output.

---

### **Step 3: Search for Software Packages Using Chocolatey**

1. **Search for Software Packages**:
   - Use Chocolatey to search for available software packages. For example, if you want to search for **Node.js**, run the following command:

   ```powershell
   choco search nodejs
   ```

2. **Get Package Information**:
   - To get more information about a specific package, including versions and dependencies, run the following command:

   ```powershell
   choco info nodejs
   ```

   - The output will provide detailed information about the package, such as its description, dependencies, and available versions.

---

### **Step 4: Automate Multiple Software Installations**

1. **Install Multiple Software Packages**:
   - You can install multiple software packages at once using Chocolatey. For example, let’s install **Google Chrome**, **7-Zip**, and **Notepad++** in a single command:

   ```powershell
   choco install googlechrome 7zip notepadplusplus -y
   ```

2. **Verify Installation**:
   - Once the installation is complete, verify the installed software by checking for the applications in the **Start Menu** or by running their respective commands in PowerShell.

---

### **Step 5: Manage and Update Installed Software Packages**

1. **Check for Package Updates**:
   - To see if any of your installed packages have updates available, run the following command:

   ```powershell
   choco outdated
   ```

   - The output will display any outdated packages.

2. **Update a Specific Package**:
   - If you have outdated packages, update one by running the following command (for example, updating **Git**):

   ```powershell
   choco upgrade git -y
   ```

3. **Uninstall a Package**:
   - To remove a package, for example **Notepad++**, run the following command:

   ```powershell
   choco uninstall notepadplusplus -y
   ```

---

### **Step 6: Work with Chocolatey Configuration and Dependencies**

1. **Install a Package with Dependencies**:
   - Chocolatey handles dependencies for you. For example, install **Node.js**, which has dependencies:

   ```powershell
   choco install nodejs -y
   ```

   - Chocolatey will automatically install any dependencies required by Node.js.

2. **Modify Chocolatey Configuration**:
   - Chocolatey allows you to configure various settings. To view the current configuration, run:

   ```powershell
   choco config
   ```

   - To modify a configuration setting (for example, enabling proxy caching), use:

   ```powershell
   choco config set cacheLocation C:\choco_cache
   ```

---

### **Lab Summary**

In this lab, you installed Chocolatey on Windows Server 2022 and learned how to use it to manage software packages. You installed, updated, and removed software using simple commands. You also explored advanced package management features such as handling dependencies, automating multiple installations, and configuring Chocolatey settings. This experience helps streamline software management on Windows servers and can be applied to various real-world scenarios.
