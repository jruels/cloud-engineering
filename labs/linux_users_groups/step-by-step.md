### Managing Users and Groups

---

### **Step 1: Access the Server and Gain Root Privileges**

1. **Connect to the server using SSH:**

   Connect to the server using `ssh` or `Putty`

   

2. **Become the root user:**
   - Once connected, switch to the root user to gain administrative privileges:

   ```bash
   sudo su - 
   ```

   - Enter the password if prompted. You are now working as the root user.

---

### **Step 2: Add Users to the Server**

1. **Add user `tstark`:**
   - Use the `useradd` command to create the first user:

   ```bash
   useradd tstark
   ```

2. **Add user `cdanvers`:**
   - Add the second user:

   ```bash
   useradd cdanvers
   ```

3. **Add user `dprince`:**
   - Finally, add the third user:

   ```bash
   useradd dprince
   ```

   - You have now successfully added the three users to the system.

---

### **Step 3: Create a New Group**

1. **Create the `superhero` group:**
   - Use the `groupadd` command to create a new group named `superhero`:

   ```bash
   groupadd superhero
   ```

   - This new group will be used to provide supplementary privileges to users.

---

### **Step 4: Change the Primary Group for `tstark`**

1. **Change the primary group to `wheel`:**
   - Use the `usermod` command to change `tstark`'s primary group to `wheel`:

   ```bash
   usermod -g wheel tstark
   ```

2. **Verify the change:**
   - Check `tstark`'s current group memberships by running the `id` command:

   ```bash
   id tstark
   ```

   - The output should show that `tstark`'s primary group is now `wheel`.

---

### **Step 5: Add Supplementary Group Memberships**

1. **Add `tstark` to the `superhero` group:**
   - Use the `usermod` command with the `-aG` option to add `tstark` to the `superhero` group without removing him from other groups:

   ```bash
   usermod -aG superhero tstark
   ```

2. **Add `cdanvers` to the `superhero` group:**
   - Add `cdanvers` as a member of the `superhero` group:

   ```bash
   usermod -aG superhero cdanvers
   ```

3. **Add `dprince` to the `superhero` group:**
   - Similarly, add `dprince` to the `superhero` group:

   ```bash
   usermod -aG superhero dprince
   ```

4. **Verify the changes:**
   - Verify the group membership for any of the users by running the `id` command. For example, check `tstark`:

   ```bash
   id tstark
   ```

   - The output should show that `tstark` is now part of both the `wheel` group and the `superhero` group. Repeat this command for the other users as needed.

---

### **Step 6: Lock the `dprince` Account**

1. **Lock the `dprince` account:**
   - Use the `usermod` command with the `-L` option to lock the `dprince` account:

   ```bash
   usermod -L dprince
   ```

   - This will prevent `dprince` from logging into the system.

2. **Verify the account lock:**
   - Check the status of the `dprince` account by attempting to log in with it or by running the following command:

   ```bash
   passwd -S dprince
   ```

   - The output should indicate that the account is locked.

---

### **Conclusion**

You have now completed the tasks for managing users and groups on an Ubuntu 20.04 system. This included adding users, creating and assigning groups, modifying user permissions, and locking an account. These skills are fundamental for maintaining system security and user management in a Linux environment.