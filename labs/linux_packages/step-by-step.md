# Install and manage Linux packages

### Lab Overview

In this lab, students will download and install the **Cowsay** package, explore the files installed by the package, and investigate where the binary is located. Students will also explore the `$PATH` environment variable and learn more about interacting with packages on Ubuntu using the `apt` package manager.

---

### Step 0: Create and log into an Ubuntu VM

Create and log into an Ubuntu VM with the username `ubuntu`



### Step 1: Download the Cowsay Package

1. **Use `wget` to download the Cowsay `.deb` file**:
   First, download the Cowsay package from the Ubuntu repository using `wget`:

   ```bash
   wget http://archive.ubuntu.com/ubuntu/pool/universe/c/cowsay/cowsay_3.03+dfsg2-7_all.deb
   ```

2. **Verify the downloaded file**:
   Confirm the package was downloaded successfully by listing the current directory:

   ```bash
   ls
   ```

   You should see `cowsay_3.03+dfsg2-7_all.deb` in the output.

---

### Step 2: Install Cowsay with `dpkg`

1. **Install the Cowsay package using `dpkg`**:
   Use the `dpkg` tool to install the package manually:

   ```bash
   sudo dpkg -i cowsay_3.03+dfsg2-7_all.deb
   ```

   This will install the package on your system.

2. **Fix any dependency issues (if needed)**:
   If any missing dependencies are reported after installing, you can resolve them using `apt`:

   ```bash
   sudo apt-get install -f
   ```

   This command will automatically install any dependencies that may have been missed.

---

### Step 3: List the Files Installed by the Package

1. **List the files contained in the Cowsay package**:
   Use the following command to list all files that were installed as part of the Cowsay package:

   ```bash
   dpkg -L cowsay
   ```

   This will show you all the files installed by the package, including configuration files, binaries, and other resources.

---

### Step 4: Find the Binary Location Using `whereis`

1. **Locate the Cowsay binary**:
   Use the `whereis` command to locate the binary file for Cowsay:

   ```bash
   whereis cowsay
   ```

   The output will show you where the `cowsay` binary is installed on your system (typically in `/usr/games/cowsay`).

---

### Step 5: Investigate the `$PATH` Environment Variable

1. **Check the current value of your `$PATH`**:
   The `$PATH` environment variable tells the system where to look for executables. Run the following command to see the directories included in your `$PATH`:

   ```bash
   echo $PATH
   ```

   This will output a list of directories where the system searches for executable files.

2. **Ensure `/usr/games` is in your `$PATH`**:
   Check if `/usr/games` is included in your `$PATH`, since that’s where the Cowsay binary is typically located.

   If `/usr/games` is missing from your `$PATH`, you can temporarily add it with:

   ```bash
   export PATH=$PATH:/usr/games
   ```

   This will allow you to run the `cowsay` command directly without specifying its full path.

---

### Step 6: Test Cowsay

1. **Run Cowsay**:
   Use the `cowsay` command to print a fun message. For example:

   ```bash
   cowsay "Hello, world!"
   ```

   You should see a cow in your terminal saying "Hello, world!".

---

### Step 7: Learn More About the `apt` Package Manager

1. **Update the package index**:
   The first thing you should always do before using `apt` is update the package index to ensure you have the latest information:

   ```bash
   sudo apt update
   ```

2. **Search for a package using `apt`**:
   Use `apt` to search for a package by its name. For example, search for the `cowsay` package:

   ```bash
   apt search cowsay
   ```

   This will show you information about the `cowsay` package and related packages available in the repository.

3. **Get detailed information about the Cowsay package**:
   You can get detailed information about the Cowsay package using:

   ```bash
   apt show cowsay
   ```

   This will provide version details, dependencies, and other metadata about the package.

4. **Remove the Cowsay package**:
   If you wish to remove the package, you can use the following command:

   ```bash
   sudo apt remove cowsay
   ```

5. **List Installed Packages**:
   Use the following command to list all installed packages on your system:

   ```bash
   dpkg -l
   ```

   This will show a complete list of packages installed on your machine.

---

### Additional Exploration (Optional)

1. **Find dependencies of a package**:
   To see the dependencies for a package, use the following command:

   ```bash
   apt depends cowsay
   ```

   This will list all the packages that `cowsay` depends on to run.

2. **Find reverse dependencies**:
   To see what packages depend on `cowsay`, use:

   ```bash
   apt rdepends cowsay
   ```

3. **Clean up downloaded package files**:
   After downloading and installing packages, you might want to clean up to free up space:

   ```bash
   sudo apt clean
   ```

---

### Summary

In this lab, you learned how to manually download and install a package using `wget` and `dpkg` in Ubuntu 20.04. You explored the files installed by the Cowsay package, located its binary using `whereis`, and investigated your `$PATH` to ensure the system can find executables. Additionally, you gained more experience using the `apt` package manager to search for packages, view detailed package information, and manage package installations. By the end of the lab, you should have a deeper understanding of package management in Ubuntu.