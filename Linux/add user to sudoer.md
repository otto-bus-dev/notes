
To add a user as a sudoer using the **wheel** group (common on many Linux distributions, especially those using `sudo` and based on Red Hat/Fedora), follow these steps:

### 1. Add the User to the Wheel Group

Replace `username` with your actual username:

sh

```
sudo usermod -aG wheel username
```

- `-aG wheel`: Adds the user to the wheel group without removing them from other groups.

### 2. Ensure Wheel Group Has Sudo Privileges

Check that your `/etc/sudoers` file allows members of the wheel group to use `sudo`.  
Open it safely with:

sh

```
sudo visudo
```

Look for this line (uncomment it if needed):

Code

```
%wheel  ALL=(ALL)       ALL
```

This line means: users in the wheel group can run any command as any user via `sudo`.

### 3. Test the Setup

Log in as the user, or switch users:

sh

```
su - username
```

Test if you can use sudo:

sh

```
sudo whoami
```

If configured properly, it should output:

Code

```
root
```

---

**Summary:**

- Add user to wheel: `sudo usermod -aG wheel username`
- Confirm wheel group is in `/etc/sudoers`
- Test with `sudo` command

Let me know if you need instructions for a different Linux distribution or user management tool!