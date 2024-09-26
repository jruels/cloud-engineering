### Managing Users and Groups

---

### **Lab Overview**

In this lab, you will solve challenges related to user and group management on an Ubuntu server. User management is an essential skill for securing a server, as it ensures that only authorized individuals can access specific resources. You will add users, create groups, modify user group assignments, and lock accounts to gain practical experience with managing user permissions.

---

### **Learning Objectives**

- Connect to a remote server using SSH.
- Add new users to the system using the `useradd` command.
- Create and manage groups using `groupadd` and `usermod`.
- Modify primary and supplementary group assignments for users.
- Lock a user account using `usermod`.

---

### **Problem 1: Access the Server and Gain Root Privileges**

#### Scenario:
To begin managing users and groups, you must first connect to the server via SSH and switch to the root user for administrative privileges.

**Tasks:**
- Connect to the server using the provided credentials via SSH.
- Become the root user by using `sudo`.

---

### **Problem 2: Add Users to the Server**

#### Scenario:
Your next task is to add three users to the system. These users will later be assigned to groups and managed accordingly.

**Tasks:**

- Add the following users to the system:
  - `tstark`
  - `cdanvers`
  - `dprince`

---

### **Problem 3: Create a New Group**

#### Scenario:
Now that you've added users, you must create a new group called `superhero`. This group will be used to assign supplementary permissions to users.

**Tasks:**
- Create a new group named `superhero` using the appropriate command.

---

### **Problem 4: Change the Primary Group for a User**

#### Scenario:
By default, each user has their own private group as the primary group. You will now modify the primary group for `tstark` to be the `wheel` group, which grants administrative privileges on many systems.

**Tasks:**
- Change `tstark`'s primary group to `wheel` using the `usermod` command.
- Verify that the change was successful by checking the user's group membership with the `id` command.

---

### **Problem 5: Add Supplementary Group Memberships**

#### Scenario:
In addition to their primary groups, the users need to be part of the `superhero` group to gain supplementary privileges. You will now add `tstark`, `cdanvers`, and `dprince` to the `superhero` group as secondary group memberships.

**Tasks:**
- Use the `usermod` command to add `tstark`, `cdanvers`, and `dprince` to the `superhero` group.
- Verify the changes by using the `id` command to check that each user has been added to the `superhero` group.

---

### **Problem 6: Lock the `dprince` Account**

#### Scenario:
For security reasons, the `dprince` account needs to be locked to prevent the user from logging in. You will accomplish this by using the `usermod` command to lock the account.

**Tasks:**
- Lock the `dprince` account using the `usermod -L` command.
- Verify that the account is locked by attempting to check the account status or attempting to log in.

---

### **Lab Summary**

In this lab, you managed users and groups on an Ubuntu system. You added users to the system, created new groups, and modified user memberships by changing primary groups and adding supplementary groups. Additionally, you locked a user account to restrict access. These tasks gave you hands-on experience with necessary system administration utilities like `useradd`, `groupadd`, and `usermod`, enhancing your ability to secure and manage a Linux environment.