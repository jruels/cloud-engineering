# Install and manage Linux packages

### **Lab Overview**

In this lab, you will solve a series of tasks related to downloading, installing, and managing the **Cowsay** package on Ubuntu 20.04. You will explore how to manually download a `.deb` file, install it using `dpkg`, and investigate where the package files are installed. Additionally, you will explore how `$PATH` works, and how to search for and manage packages using the `apt` package manager.

---

### **Learning Objectives**

- Create an Ubuntu EC2 instance
- Download and install a `.deb` package using `wget` and `dpkg`.
- Explore package files and binaries using Linux commands like `dpkg` and `whereis`.
- Investigate and modify the `$PATH` environment variable to ensure executables are available.
- Interact with the `apt` package manager to search for, install, and remove packages.

---

### **Problem 0: Create and log into an Ubuntu EC2 instance**

Create and log into an Ubuntu EC2 instance with the username `ubuntu`

### **Problem 1: Download the Cowsay Package**

#### Scenario:
Your first task is to download the **Cowsay** `.deb` package from the Ubuntu repository and verify that the file has been successfully downloaded.

**Tasks:**
- Use `wget` to download the Cowsay package from the following URL:  
  `http://archive.ubuntu.com/ubuntu/pool/universe/c/cowsay/cowsay_3.03+dfsg2-7_all.deb`
- List the contents of your current directory to ensure the `.deb` file is downloaded.

---

### **Problem 2: Install Cowsay Using `dpkg`**

#### Scenario:
Once the package has been downloaded, your next task is to manually install it using the `dpkg` tool. If there are any missing dependencies, you'll need to resolve them.

**Tasks:**
- Install the Cowsay package using `dpkg`.
- If any dependency issues arise, fix them using the `apt` package manager to install the missing dependencies.

---

### **Problem 3: List Files Installed by the Package**

#### Scenario:
Now that Cowsay is installed, you need to explore the files that were installed as part of the package.

**Tasks:**
- Use the `dpkg` command to list all the files installed by the Cowsay package.
- Identify any important files such as configuration files or binaries.

---

### **Problem 4: Locate the Cowsay Binary**

#### Scenario:
To execute the Cowsay command, you need to locate where the binary is installed on the system.

**Tasks:**
- Use the `whereis` command to find the location of the `cowsay` binary.
- Verify that the binary is located in `/usr/games`.

---

### **Problem 5: Investigate and Modify the `$PATH` Environment Variable**

#### Scenario:
The `$PATH` environment variable is used by the system to locate executables. In this task, you'll check your current `$PATH` and ensure that `/usr/games` is included so you can run the `cowsay` command from anywhere in your terminal.

**Tasks:**
- Check your current `$PATH` variable.
- Ensure `/usr/games` is included in your `$PATH`.
- If `/usr/games` is missing, temporarily add it to your `$PATH`.
- Optionally, make the change permanent by modifying your `.bashrc` or `.bash_profile`.

---

### **Problem 6: Test Cowsay**

#### Scenario:
Now that the package is installed and your `$PATH` is set correctly, you can run the `cowsay` command to test that everything is working as expected.

**Tasks:**
- Run the `cowsay` command to print a message. For example, have the cow say "Hello, world!".

---

### **Problem 7: Explore the `apt` Package Manager**

#### Scenario:
You now need to explore some of the advanced features of the `apt` package manager, such as searching for packages, showing detailed information, and removing installed packages.

**Tasks:**
- Update the package index to ensure you have the latest package information.
- Use `apt` to search for the `cowsay` package and review its details.
- Get detailed package information for Cowsay, including version, dependencies, and description.
- Remove the Cowsay package from your system using `apt`.
- List all installed packages on your system using `dpkg`.

---

### **Additional Exploration (Optional)**

#### Scenario:
To deepen your understanding of package management, you can explore package dependencies and how to clean up unnecessary package files from your system.

**Tasks:**
- Find out which packages `cowsay` depends on using the `apt depends` command.
- Identify packages that depend on `cowsay` using the `apt rdepends` command.
- Clean up downloaded package files using `apt clean` to free up disk space.

---

### **Lab Summary**

In this lab, you created an Ubuntu EC2 instances, manually downloaded and installed the Cowsay package using `wget` and `dpkg`, explored the files installed by the package, and located the Cowsay binary. You also worked with the `$PATH` environment variable to ensure that system executables are easily accessible. Finally, you explored the `apt` package manager by searching for packages, viewing detailed information, and removing installed packages. These tasks provided a deeper understanding of package management on Ubuntu 20.04 and helped you develop essential system administration skills.