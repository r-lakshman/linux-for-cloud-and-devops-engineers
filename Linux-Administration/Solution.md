# Linux System Administration & AWS Infrastructure Management - Project Guide

## Prerequisites & Connection Guide

Log in to the AWS Cloud and create a Linux-based Amazon EC2 instance to complete the project tasks.

### 1. Launch EC2 Instance
Navigate to the **AWS Management Console $\rightarrow$ EC2 $\rightarrow$ Launch Instance** and configure the following settings:

* **Name:** `linux-devops-lab`
* **AMI (Amazon Machine Image):** Amazon Linux 2023
* **Architecture:** 64-bit (x86)
* **Instance Type:** `t3.micro`
* **Root Volume:** 8 GiB `gp3`
* **Additional EBS Volume:** None
* **Key Pair:** `linux-devops-key`
* **Key Format:** `.pem`

#### Network Settings
* Allow SSH (port 22).
* Restrict SSH to your trusted IP address.
* Never use unrestricted SSH access (`0.0.0.0/0`) for this lab.

#### Advanced Recommendation
If you prefer AWS Systems Manager Session Manager, attach an EC2 IAM role with `AmazonSSMManagedInstanceCore` and use Session Manager instead of managing a private SSH key.

Click **Launch Instance** to create your virtual machine.

### 2. Connection Methods
You can connect to your instance using any of the following supported methods:
* **Browser-Based Tools:** EC2 Instance Connect or AWS Systems Manager (SSM) Session Manager
* **SSH Clients:** Terminal (Linux/macOS), PuTTY, or MobaXterm (Windows)

#### Connecting via Terminal (SSH)
1. Locate or download your private key file (`linux-devops-key.pem`).
2. Restrict the file permissions so your private key is not publicly viewable:
   ```bash
   chmod 400 "linux-devops-key.pem"

---

# Module 1: Login to the server as super user and perform below
- Create users and set passwords – `user1`, `user2`, `user3`
- Create Groups – `devops`, `aws`
- Change primary group of `user2`, `user3` to `devops` group
- Add `aws` group as secondary group to the `user1`
- Create the file and directory structure shown in the above diagram.
- Change group of `/dir1`, `/dir7/dir10`, `/f2` to `devops` group
- Change ownership of `/dir1`, `/dir7/dir10`, `/f2` to `user1` user.

## Verify your Linux environment
- `whoami`
- `hostname`
- `pwd`
- `df -h`

## Switch to root user
- `sudo -i`
  - Log in as root
- `whoami`
  ```bash
  root
  ```
- **Note:**
  - `sudo` (Superuser do) $\rightarrow$ Borrow admin power (uses your password).
  - `su` (Switch user or substitute user) $\rightarrow$ Become another user entirely (uses their password).

## Using `sudo !!` in Linux
- If you execute a command that requires sudo privileges without using sudo, the command may fail with a permission error.
  - Instead of typing the entire command again with sudo, you can simply use:
  - `sudo !!`
    - `!!` is a Linux shell shortcut called last command expansion.
    - It represents the previous command you executed.
    - `sudo !!` reruns the previous command with sudo privileges.

## Giving Users Passwordless Sudo Access in Linux
- Normally, when we add a user to the wheel group, the user can execute commands with sudo privileges, but they may be prompted to enter their password.
- If we want to allow a user to execute sudo commands without entering a password every time, we can configure passwordless sudo access.
  - **Step 1: Add the user to the wheel group**
    ```bash
    usermod -aG wheel user
    ```
  - **Step 2: Configure passwordless sudo access**
    ```bash
    echo 'user ALL=(ALL) NOPASSWD: ALL' > /etc/sudoers.d/user
    ```

---

## Step 1: Create the first three users
- Create users and set passwords – `user1`, `user2`, `user3`.
  ```bash
  useradd user1
  useradd user2
  useradd user3
  ```
- Set password individually
  ```bash
  passwd user1
  passwd user2
  passwd user3
  ```
- Check all the users
  ```bash
  who
  who | wc -l
  ```
- Verify Users
  ```bash
  id user1
  id user2
  id user3
  ```
- Check users
  ```bash
  cat /etc/passwd
  cut -d: -f1 /etc/passwd
  ```

---

## Step 2: Create the groups
- `devops`
- `aws`

- Run
  ```bash
  groupadd devops
  groupadd aws
  ```
- Verify with
  ```bash
  getent group devops
  getent group aws
  ```

---

## Step 3: Change primary group of `user2`, `user3` to `devops` group
- Change the primary group of `user2` to `devops`.
  ```bash
  usermod -g devops user2
  ```
- Change the primary group of `user3` to `devops`.
  ```bash
  usermod -g devops user3
  ```
- Verify with
  ```bash
  id user2
  id user3
  ```
  - `-g` $\rightarrow$ Changes primary group

---

## Step 4: Add `aws` group as secondary group to the `user1`
- Add `aws` group as secondary group to the `user1`
  ```bash
  usermod -aG aws user1
  ```
  - `-G` $\rightarrow$ Sets supplementary groups
  - `-aG` $\rightarrow$ Adds a user to a supplementary group without removing the user's existing group memberships.
- Verify with
  ```bash
  id user1
  ```

---

## Step 5: Create the file and directory structure shown in the above diagram.

### Create all folders
```bash
mkdir -p home \
dir1 \
dir2/dir1/dir2/dir10 \
dir3 \
dir4 \
dir5 \
dir6 \
dir7/dir10 \
dir8 \
opt/dir14/dir10
```

### View all folders
- ```bash
  ls -d /dir* /opt/dir14
  ```

### View the complete folder structure
- ```bash
  find /dir1 /dir2 /dir3 /dir4 /dir5 /dir6 /dir7 /dir8 /opt/dir14 -type d
  ```

### Create the files
```bash
touch dir1/f1 \
      f1 \
      f2 \
      dir2/dir1/dir2/f3 \
      dir4/dir12/f5 \
      dir4/dir12/f4 \
      dir7/f3 \
      opt/dir14/f3
```

---

## Step 6: Change group of `/dir1`, `/dir7/dir10`, `/f2` to `devops` group
- ```bash
  chgrp devops /dir1
  chgrp devops /dir7/dir10
  chgrp devops /f2
  ```

---

## Step 7: Change ownership of `/dir1`, `/dir7/dir10`, `/f2` to `user1` user.
- ```bash
  chown user1 /dir1
  chown user1 /dir7/dir10
  chown user1 /f2
  ```
  - Or one line:
    ```bash
    chown user1 /dir1 && chown user1 /dir7/dir10 && chown user1 f2
    ```
- Verify with:
  ```bash
  ls -ld /dir1 /dir7/dir10 /f2
  ```
---

# Module 2: User and Group Management as user1

## Objective

Log in to the Linux server as `user1` and perform the following tasks:

- Create users: `user4` and `user5`.
- Set passwords for both users.
- Create groups: `app` and `database`.
- Verify the users and groups.

## Step 1: Grant Sudo Access to user1

On Amazon Linux, log in as root and add `user1` to the `wheel` group.

```bash
usermod -aG wheel user1
```

Log out and log back in as `user1` for the new group membership to apply.

## Step 2: Login as user1

Switch to user1:

```bash
su - user1
```

Verify the current user:

```bash
whoami
```

Expected output:

```text
user1
```

**Note:** User1 needs administrative privileges because creating users and groups requires root-level permissions.

## Step 3: Configure Passwordless Sudo (Optional)

If you want user1 to execute sudo commands without entering a password, run:

```bash
echo "user1 ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/user1
```

Set the correct permissions:

```bash
sudo chmod 440 /etc/sudoers.d/user1
```

Validate the sudoers configuration:

```bash
sudo visudo -c
```

### Verify sudo access

```bash
sudo -k
whoami
```

Expected output:

```text
user1
```

Now execute:

```bash
sudo whoami
```

Expected output:

```text
root
```

## Step 4: Execute Selected Administrative Commands Without Typing sudo

If you want to run selected administrative commands without explicitly typing `sudo`, you can create shell functions in your `.bashrc` file.

Add the following functions:

```bash
echo 'useradd(){ sudo /usr/sbin/useradd "$@"; }' >> ~/.bashrc
echo 'groupadd(){ sudo /usr/sbin/groupadd "$@"; }' >> ~/.bashrc
echo 'userdel(){ sudo /usr/sbin/userdel "$@"; }' >> ~/.bashrc
echo 'passwd(){ sudo /usr/bin/passwd "$@"; }' >> ~/.bashrc
```

Reload the shell configuration:

```bash
source ~/.bashrc
```

**Remember:**

- `.bashrc` stores shell configuration.
- Shell functions allow you to reuse commands.
- These functions internally execute the commands using sudo.
- Passwordless sudo must be configured for commands to run without password prompts.

### Test the useradd function

Create a test user:

```bash
useradd -m user7
```

Verify:

```bash
id user7
```

Delete the test user:

```bash
userdel -r user7
```

**Note:** The `-m` option creates the user's home directory. The `-r` option removes the user's home directory and mail spool when deleting the user.

## Step 5: Create Users

Create user4:

```bash
useradd -m user4
```

Create user5:

```bash
useradd -m user5
```

## Step 6: Set Passwords

Set the password for user4:

```bash
passwd user4
```

Set the password for user5:

```bash
passwd user5
```

Enter and confirm the passwords when prompted.

## Step 7: Verify Users

Check user4:

```bash
id user4
```

Check user5:

```bash
id user5
```

You can also verify their account entries:

```bash
getent passwd user4
getent passwd user5
```

## Step 8: Create Groups

Create the `app` group:

```bash
groupadd app
```

Create the `database` group:

```bash
groupadd database
```

## Step 9: Verify Groups

Check the app group:

```bash
getent group app
```

Check the database group:

```bash
getent group database
```

## Step 10: Final Verification Checklist

- [ ] Logged in as user1.
- [ ] Added user1 to the wheel group.
- [ ] Verified sudo access.
- [ ] Configured passwordless sudo (optional).
- [ ] Created and tested shell functions (optional).
- [ ] Created user4.
- [ ] Created user5.
- [ ] Set passwords for user4 and user5.
- [ ] Verified user4 and user5.
- [ ] Created app group.
- [ ] Created database group.
- [ ] Verified both groups.

## Useful Commands Summary

| Command | Purpose |
|---|---|
| `su - user1` | Switch to user1 |
| `usermod -aG wheel user1` | Add user1 to wheel |
| `sudo whoami` | Verify sudo privileges |
| `useradd -m` | Create user with home directory |
| `passwd` | Set user password |
| `groupadd` | Create a group |
| `id` | Display user and group information |
| `getent passwd` | Verify user account |
| `getent group` | Verify group |
| `userdel -r` | Delete user and home directory |
| `source ~/.bashrc` | Reload shell configuration |
| `visudo -c` | Validate sudoers configuration |

---

# Module 3: File and Directory Management as user4

## Objective

Log in to the Linux server as `user4` and perform the following tasks:

1. Create directory: `/dir6/dir4`
2. Create file: `/f3`
3. Move the file `/dir1/f1` to `/dir2/dir1/dir2`
4. Rename the file `/f2` to `/f4`

## Step 1: Add user4 to the wheel Group

Before logging in as user4, log in as user1 and execute:

```bash
sudo usermod -aG wheel user4
```

### Verify Group Membership

```bash
id user4
```

You should see `wheel` in the supplementary groups list.

**Note:** Log out and log back in as user4 after changing group membership so the new membership takes effect.

## Step 2: Login as user4

Switch to user4:

```bash
su - user4
```

Enter the user4 password when prompted.

### Verify Login

```bash
whoami
```

Expected output:

```text
user4
```

Verify sudo access:

```bash
sudo whoami
```

Expected output:

```text
root
```

## Step 3: Create Directory /dir6/dir4

Create the directory:

```bash
sudo mkdir -p /dir6/dir4
```

### Verify Directory

```bash
ls -ld /dir6/dir4
```

Alternatively:

```bash
sudo find /dir6 -type d
```

**Explanation:**

- `mkdir`: Creates a directory.
- `-p`: Creates parent directories if they do not already exist.

## Step 4: Create File /f3

Create the file:

```bash
sudo touch /f3
```

### Verify File

```bash
ls -l /f3
```

**Note:** Creating files directly under the root directory requires appropriate permissions, so sudo is used.

## Step 5: Move File /dir1/f1 to /dir2/dir1/dir2

Move the file:

```bash
sudo mv /dir1/f1 /dir2/dir1/dir2/
```

### Verify the Moved File

```bash
ls -ltr /dir2/dir1/dir2
```

Check the original location:

```bash
ls -l /dir1/
```

The file `f1` should no longer exist in `/dir1`.

**Explanation:**

- `mv`: Moves or renames files and directories.
- The destination directory must exist.
- The trailing slash indicates that the destination is a directory.

## Step 6: Rename File /f2 to /f4

Rename the file:

```bash
sudo mv /f2 /f4
```

### Verify the Rename

```bash
ls -l /f4
```

Check that the old filename no longer exists:

```bash
ls -l /f2
```

The second command should report that the file does not exist.

**Explanation:**

The `mv` command is used to rename files in Linux. When the source and destination are in the same directory, it renames the file.

## Step 7: Final Verification

Run the following commands:

```bash
ls -ld /dir6/dir4
ls -l /f3
ls -l /dir2/dir1/dir2/f1
ls -l /f4
```

Expected results:

| Task | Expected Result |
|---|---|
| Create /dir6/dir4 | Directory exists |
| Create /f3 | File exists |
| Move /dir1/f1 | File exists at /dir2/dir1/dir2/f1 |
| Rename /f2 to /f4 | /f4 exists |

## Step 8: Final Verification Checklist

- [ ] Added user4 to the wheel group.
- [ ] Logged in as user4.
- [ ] Verified sudo access.
- [ ] Created /dir6/dir4.
- [ ] Created /f3.
- [ ] Moved /dir1/f1 to /dir2/dir1/dir2.
- [ ] Renamed /f2 to /f4.
- [ ] Verified all directories and files.

## Useful Linux Commands Summary

| Command | Purpose |
|---|---|
| `usermod -aG wheel user4` | Add user4 to wheel |
| `su - user4` | Switch to user4 |
| `sudo whoami` | Verify sudo access |
| `mkdir -p` | Create directories recursively |
| `touch` | Create an empty file |
| `mv` | Move or rename files |
| `ls -l` | List file details |
| `ls -ld` | Display directory details |
| `find` | Search files and directories |

---

# Module 4: Advanced File and Directory Operations as user1

## Objective

Log in to the Linux server as `user1` and perform the following tasks:

1. Create directory: `/home/user2/dir1`.
2. Change to `/dir2/dir1/dir2/dir10` and create `/opt/dir14/dir10/f1` using a relative path.
3. Move the file `/opt/dir14/dir10/f1` to the home directory of user1.
4. Delete the directory `/dir4` recursively.
5. Delete all child files and directories under `/opt/dir14` using a single command.
6. Write the specified text into `/f3` and save it.

## Step 1: Login as user1

Switch to user1:

```bash
su - user1
```

Enter the user1 password when prompted.

Verify the current user:

```bash
whoami
```

Expected output:

```text
user1
```

## Step 2: Create Directory /home/user2/dir1

Create the directory:

```bash
mkdir -p /home/user2/dir1
```

If user1 does not have permission to create directories under user2's home directory, use:

```bash
sudo mkdir -p /home/user2/dir1
```

### Verify

```bash
ls -ld /home/user2/dir1
```

**Explanation:**

- `mkdir`: Creates a directory.
- `-p`: Creates parent directories if they do not exist.

## Step 3: Create a File Using a Relative Path

First, change to the specified directory:

```bash
cd /dir2/dir1/dir2/dir10
```

Verify the current working directory:

```bash
pwd
```

Expected output:

```text
/dir2/dir1/dir2/dir10
```

Now create the file using a relative path:

```bash
sudo touch ../../../../opt/dir14/dir10/f1
```

### Understanding Relative Paths

Current directory:

```text
/dir2/dir1/dir2/dir10
```

Each `..` moves one directory level upward.

Path traversal:

```text
/dir2/dir1/dir2/dir10
/dir2/dir1/dir2
/dir2/dir1
/dir2
/
```

After moving four levels upward, the path starts from the root directory.

Therefore:

```text
../../../../opt/dir14/dir10/f1
```

Refers to:

```text
/opt/dir14/dir10/f1
```

### Verify File Creation

```bash
sudo ls -l /opt/dir14/dir10/f1
```

## Step 4: Move the File to user1 Home Directory

Move the file:

```bash
sudo mv /opt/dir14/dir10/f1 /home/user1/
```

### Verify

```bash
ls -l /home/user1/f1
```

Check the original location:

```bash
sudo ls -l /opt/dir14/dir10/
```

The file `f1` should no longer exist in the original directory.

**Explanation:**

- `mv`: Moves or renames files and directories.
- `/home/user1/`: Destination directory.

## Step 5: Delete the Directory /dir4 Recursively

Execute:

```bash
sudo rm -rf /dir4
```

### Verify

```bash
ls -ld /dir4
```

The command should report that the directory does not exist.

**Explanation:**

- `rm`: Removes files and directories.
- `-r`: Removes directories recursively, including their contents.
- `-f`: Forces removal without interactive confirmation.

**Warning:** The `rm -rf` command permanently removes the specified directory and its contents. Always verify the path before executing it.

## Step 6: Delete All Contents Under /opt/dir14 Using a Single Command

Execute:

```bash
sudo find /opt/dir14 -mindepth 1 -delete
```

### Verify

```bash
sudo ls -la /opt/dir14
```

Only the parent directory `/opt/dir14` should remain. Its child files, directories, and hidden contents should be deleted.

**Explanation:**

- `find`: Searches files and directories.
- `/opt/dir14`: Starting directory.
- `-mindepth 1`: Excludes the starting directory itself.
- `-delete`: Deletes all matching files and directories.

**Important:** This command preserves `/opt/dir14` but deletes everything inside it, including hidden files and directories.

## Step 7: Write Text to the /f3 File

Required text:

```text
Linux assessment for a DevOps Engineer!! Learn with Fun!!
```

Write the text into `/f3`:

```bash
echo "Linux assessment for a DevOps Engineer!! Learn with Fun!!" | sudo tee /f3
```

### Verify File Content

```bash
cat /f3
```

Expected output:

```text
Linux assessment for a DevOps Engineer!! Learn with Fun!!
```

**Explanation:**

- `echo`: Displays the specified text.
- `|`: Sends the output to another command.
- `sudo tee`: Writes the received text into a file with elevated privileges.
- `cat`: Displays file contents.

## Step 8: Final Verification

Run the following commands:

```bash
ls -ld /home/user2/dir1
ls -l /home/user1/f1
ls -ld /dir4
sudo ls -la /opt/dir14
cat /f3
```

Expected results:

| Task | Expected Result |
|---|---|
| Create /home/user2/dir1 | Directory exists |
| Create file using relative path | File created successfully |
| Move f1 | File exists in /home/user1 |
| Delete /dir4 | Directory does not exist |
| Delete contents of /opt/dir14 | Parent remains empty |
| Write text to /f3 | Required text is displayed |

## Step 9: Final Verification Checklist

- [ ] Logged in as user1.
- [ ] Created /home/user2/dir1.
- [ ] Changed to /dir2/dir1/dir2/dir10.
- [ ] Created /opt/dir14/dir10/f1 using a relative path.
- [ ] Moved f1 to /home/user1/.
- [ ] Deleted /dir4 recursively.
- [ ] Deleted all contents under /opt/dir14.
- [ ] Preserved the /opt/dir14 parent directory.
- [ ] Wrote the required text into /f3.
- [ ] Verified the file contents using cat.

## Useful Linux Commands Summary

| Command | Purpose |
|---|---|
| `su - user1` | Switch to user1 |
| `mkdir -p` | Create directories recursively |
| `cd` | Change current directory |
| `pwd` | Display current directory |
| `..` | Refer to parent directory |
| `touch` | Create an empty file |
| `mv` | Move or rename files |
| `rm -rf` | Remove directories recursively |
| `find` | Search files and directories |
| `-mindepth 1` | Exclude the starting directory |
| `-delete` | Delete matching files and directories |
| `echo` | Display text |
| `tee` | Write input into a file |
| `cat` | Display file contents |

---

# Module 5: File and Text Management as user2

## Objective

Login as user2 and perform the following tasks:

1. Create file /dir1/f2.
2. Delete /dir6.
3. Delete /dir8.
4. Replace devops with DevOps in /f3 without using an editor.
5. Using Vi Editor, copy line 1 and paste it 10 times in /f3.
6. Delete /f3.

## Step 1: Configure Sudo Access for user2

Login as root:

```bash
visudo -f /etc/sudoers.d/user2
```

Add:

```text
user2 ALL=(ALL) NOPASSWD: ALL
```

Set permissions:

```bash
chmod 440 /etc/sudoers.d/user2
```

Validate:

```bash
visudo -c
```

## Step 2: Login as user2

```bash
su - user2
```

Verify:

```bash
whoami
```

Expected output:

```text
user2
```

Verify sudo access:

```bash
sudo whoami
```

Expected output:

```text
root
```

## Step 3: Create File /dir1/f2

```bash
sudo touch /dir1/f2
```

Verify:

```bash
ls -l /dir1/f2
```

## Step 4: Delete Directories /dir6 and /dir8

Delete /dir6:

```bash
sudo rm -rf /dir6
```

Delete /dir8:

```bash
sudo rm -rf /dir8
```

Verify:

```bash
ls -ld /dir6 /dir8
```

## Step 5: Replace devops with DevOps Without Using an Editor

Use the Linux Stream Editor (sed):

```bash
sudo sed -i 's/devops/DevOps/g' /f3
```

### Explanation

- sed: Stream Editor.
- -i: Modifies the original file directly.
- s: Substitute operation.
- devops: Old text.
- DevOps: New text.
- g: Replaces all occurrences in each line.

Verify:

```bash
cat /f3
```

## Step 6: Copy Line 1 and Paste It 10 Times Using Vi Editor

Open the file:

```bash
sudo vi /f3
```

1. Press Esc to enter command mode.
2. Move to the first line:

```vim
gg
```

3. Copy line 1:

```vim
yy
```

4. Paste 10 copies below the current line:

```vim
10p
```

5. Save and exit:

```vim
:wq
```

### Verify

```bash
cat /f3
```

### Vi Editor Commands

| Command | Purpose |
|---|---|
| Esc | Enter command mode |
| gg | Move to first line |
| yy | Copy current line |
| p | Paste below current line |
| 10p | Paste 10 copies |
| :w | Save file |
| :q | Exit Vi |
| :wq | Save and exit |

## Step 7: Delete File /f3

After completing the text replacement and Vi Editor operations:

```bash
sudo rm -f /f3
```

Verify:

```bash
ls -l /f3
```

The file should no longer exist.

## Step 8: Final Verification Checklist

- [ ] Configured sudo access for user2.
- [ ] Logged in as user2.
- [ ] Verified sudo privileges.
- [ ] Created /dir1/f2.
- [ ] Deleted /dir6.
- [ ] Deleted /dir8.
- [ ] Replaced devops with DevOps using sed.
- [ ] Copied line 1 using Vi Editor.
- [ ] Pasted line 1 ten times.
- [ ] Deleted /f3.
- [ ] Verified all operations.

## Useful Linux Commands Summary

| Command | Purpose |
|---|---|
| visudo | Safely edit sudoers configuration |
| su - user2 | Switch to user2 |
| sudo whoami | Verify sudo access |
| touch | Create an empty file |
| rm -rf | Delete directories recursively |
| sed -i | Edit file contents directly |
| vi | Open Vi Editor |
| gg | Go to first line |
| yy | Copy current line |
| 10p | Paste 10 copies |
| :wq | Save and exit |
| cat | Display file contents |
| ls -l | Verify file details |

---

# Module 6: File Search and File Counting as Root

## Objective

Login to the Linux server as root and perform the following tasks:

1. Search for the file named f3 in the server and list all absolute paths where the file is found.
2. Show the count of regular files directly inside the root directory (/).
3. Print the last line of the file /etc/passwd.

## Step 1: Login as Root User

Switch to the root account:

```bash
sudo -i
```

Verify the current user:

```bash
whoami
```

Expected output:

```text
root
```

## Step 2: Search for Files Named f3

Use the find command to search the entire server for files named f3:

```bash
find / -type f -name "f3" 2>/dev/null
```

### Explanation

- `find`: Searches for files and directories.
- `/`: Starts searching from the root directory.
- `-type f`: Searches only for regular files.
- `-name "f3"`: Searches for files with the exact name f3.
- `2>/dev/null`: Hides permission and other error messages.

### Example Output

The actual output depends on which files exist on your server.

```text
/path/to/f3
/another/path/f3
```

The find command displays absolute paths because the search starts from /.

## Step 3: Count Files in the Root Directory (/)

Execute:

```bash
find / -maxdepth 1 -type f 2>/dev/null | wc -l
```

### Explanation

- `find /`: Searches starting from the root directory.
- `-maxdepth 1`: Searches only the root directory itself, not its subdirectories.
- `-type f`: Counts regular files only.
- `2>/dev/null`: Hides error messages.
- `|`: Sends the output to another command.
- `wc -l`: Counts the number of output lines.

### Example Output

```text
12
```

**Note:** The number shown above is only an example. Your actual count will depend on your Linux server.

## Step 4: Print the Last Line of /etc/passwd

Execute:

```bash
tail -n 1 /etc/passwd
```

### Explanation

- `tail`: Displays the end of a file.
- `-n 1`: Displays only the last line.
- `/etc/passwd`: Linux user account information file.

### Example Output

```text
someuser:x:1001:1001::/home/someuser:/bin/bash
```

**Note:** The actual last line depends on the user accounts configured on your server.

## Step 5: Final Verification

Run all three commands:

### Search for f3

```bash
find / -type f -name "f3" 2>/dev/null
```

### Count files in /

```bash
find / -maxdepth 1 -type f 2>/dev/null | wc -l
```

### Print the last line of /etc/passwd

```bash
tail -n 1 /etc/passwd
```

## Step 6: Final Verification Checklist

- [ ] Logged in as root.
- [ ] Searched for all regular files named f3.
- [ ] Displayed absolute file paths.
- [ ] Counted regular files directly inside /.
- [ ] Printed the last line of /etc/passwd.
- [ ] Verified all commands successfully.

## Useful Linux Commands Summary

| Command | Purpose |
|---|---|
| `sudo -i` | Switch to root |
| `whoami` | Display current username |
| `find` | Search files and directories |
| `-type f` | Search regular files |
| `-name` | Search by filename |
| `-maxdepth 1` | Limit search to one directory level |
| `2>/dev/null` | Hide error messages |
| `wc -l` | Count lines |
| `tail -n 1` | Print the last line of a file |

---

# Module 7: Create and Attach a 5 GB EBS Volume to an EC2 Instance

## Objective

Create a 5 GiB Amazon EBS volume in the same Availability Zone (AZ) as an existing EC2 instance and attach the volume to that instance.

## Step 1: Login to AWS Console

1. Open the AWS Management Console.
2. Login using your AWS credentials.
3. Navigate to the EC2 service.

## Step 2: Open Elastic Block Store

Navigate to:

```text
AWS Console → EC2 → Elastic Block Store → Volumes
```

## Step 3: Create a New EBS Volume

1. Click **Create volume**.
2. Configure the following settings:

| Setting | Value |
|---|---|
| Volume type | gp3 (General Purpose SSD) |
| Size | 5 GiB |
| Availability Zone | Same AZ as the EC2 instance |
| IOPS | Default |
| Throughput | Default |

**Important:** The EBS volume and EC2 instance must be in the same Availability Zone to attach the volume.

## Step 4: Add Tags

Under the Tags section, add the following tag:

| Key | Value |
|---|---|
| Name | EBS-Additional-Volume |

## Step 5: Create the Volume

1. Review the volume configuration.
2. Click **Create volume**.
3. Wait until the volume status becomes **Available**.

## Step 6: Attach the EBS Volume to the EC2 Instance

1. Select the newly created EBS volume.
2. Click **Actions**.
3. Select **Attach volume**.
4. Select the EC2 instance to which you want to attach the volume.
5. Select the device name.

Example device names:

```text
/dev/sdb
/dev/sdc
```

6. Click **Attach volume**.

**Note:** Use the device name offered by the AWS console. On modern Nitro-based EC2 instances, the Linux operating system may identify the attached EBS volume using an NVMe device name such as /dev/nvme1n1 rather than the requested /dev/sdb or /dev/sdc.

## Step 7: Verify Volume Attachment

Navigate to:

```text
EC2 → Elastic Block Store → Volumes
```

Select the volume and verify:

| Property | Expected Value |
|---|---|
| Volume Name | EBS-Additional-Volume |
| Size | 5 GiB |
| State | In-use |
| Attachment | Your EC2 instance ID |
| Availability Zone | Same as EC2 instance |

## Step 8: Final Verification Checklist

- [ ] Logged in to AWS Management Console.
- [ ] Opened EC2 service.
- [ ] Navigated to Elastic Block Store → Volumes.
- [ ] Created a new 5 GiB EBS volume.
- [ ] Selected the same Availability Zone as the EC2 instance.
- [ ] Added the Name tag: EBS-Additional-Volume.
- [ ] Created the EBS volume successfully.
- [ ] Selected the created volume.
- [ ] Attached the volume to the correct EC2 instance.
- [ ] Verified the volume status is In-use.

## Important Notes

- EBS volumes are Availability Zone specific.
- An EBS volume must be in the same Availability Zone as the EC2 instance to which it is attached.
- Creating and attaching an EBS volume does not automatically create a filesystem or mount it inside Linux.
- Before using the volume for storage, you must identify the device, partition or format it as appropriate, and mount it.
- Do not format a device that contains existing data.

---

# Module 8: Create Filesystem and Mount EBS Volume

## Objective

Login to the Linux server as root and perform the following tasks:

1. Identify the newly attached EBS volume.
2. Create a filesystem on the new EBS volume.
3. Create the mount directory /data.
4. Mount the EBS filesystem on /data.
5. Verify filesystem utilization using df -h.
6. Create a file named f1 inside /data.

## Step 1: Login as Root User

Switch to root:

```bash
sudo -i
```

Verify the current user:

```bash
whoami
```

Expected output:

```text
root
```

## Step 2: Identify the Newly Attached EBS Volume

List available block devices:

```bash
lsblk
```

Display filesystem information:

```bash
lsblk -f
```

Identify the newly attached 5 GiB EBS volume.

Example output:

```text
NAME          SIZE TYPE MOUNTPOINTS
nvme0n1        8G disk
├─nvme0n1p1    8G part /
nvme1n1        5G disk
```

In this example, the newly attached EBS volume is:

```text
/dev/nvme1n1
```

**Important:** Device names can differ depending on the EC2 instance type and operating system. Always identify the correct device using lsblk before proceeding.

## Step 3: Create a Filesystem on the EBS Volume

For a new, empty EBS volume, create an XFS filesystem:

```bash
mkfs.xfs /dev/nvme1n1
```

### Explanation

- `mkfs`: Creates a filesystem.
- `xfs`: Specifies the XFS filesystem type.
- `/dev/nvme1n1`: Newly attached EBS device.

### Verify Filesystem

```bash
lsblk -f
```

You should see XFS in the FSTYPE column for the new volume.

**Important Warning:** The mkfs.xfs command creates a new filesystem and destroys existing filesystem data on the selected device. Double-check the device name and ensure it is the intended empty EBS volume before executing.

## Step 4: Create the Mount Directory

Create the /data directory:

```bash
mkdir /data
```

Verify:

```bash
ls -ld /data
```

## Step 5: Mount the EBS Filesystem on /data

Mount the newly created filesystem:

```bash
mount /dev/nvme1n1 /data
```

### Explanation

- `mount`: Attaches a filesystem to a directory.
- `/dev/nvme1n1`: EBS volume device.
- `/data`: Mount point.

## Step 6: Verify Filesystem Utilization

Run:

```bash
df -h
```

Example output:

```text
Filesystem      Size  Used Avail Use% Mounted on
/dev/nvme1n1    5.0G   68M  5.0G   2% /data
```

**Note:** The actual size and used space may differ slightly depending on filesystem metadata and system configuration.

Verify only the /data mount:

```bash
df -h /data
```

The output must show /data in the Mounted on column.

## Step 7: Create File f1 in the /data Filesystem

Create the file:

```bash
touch /data/f1
```

Verify:

```bash
ls -l /data
```

Expected output:

```text
-rw-r--r-- 1 root root 0 ... f1
```

## Step 8: Final Verification

Run the following commands:

### Verify block devices

```bash
lsblk
```

### Verify filesystem type

```bash
lsblk -f
```

### Verify mount

```bash
mount | grep /data
```

### Verify disk utilization

```bash
df -h
```

### Verify file creation

```bash
ls -l /data/f1
```

## Step 9: Final Verification Checklist

- [ ] Logged in as root.
- [ ] Identified the newly attached EBS volume using lsblk.
- [ ] Verified the correct device name.
- [ ] Created an XFS filesystem.
- [ ] Verified filesystem type using lsblk -f.
- [ ] Created the /data directory.
- [ ] Mounted the EBS volume on /data.
- [ ] Verified the mount using df -h.
- [ ] Confirmed that /data appears in the Mounted on column.
- [ ] Created file /data/f1.
- [ ] Verified file creation using ls -l.

## Useful Linux Commands Summary

| Command | Purpose |
|---|---|
| sudo -i | Switch to root |
| whoami | Display current user |
| lsblk | List block devices |
| lsblk -f | Display filesystem information |
| mkfs.xfs | Create XFS filesystem |
| mkdir | Create directory |
| mount | Mount filesystem |
| df -h | Display filesystem utilization |
| df -h /data | Display /data filesystem utilization |
| touch | Create empty file |
| ls -l | List file details |
| mount \\| grep /data | Verify mount point |

---

# Module 9: Directory and File Deletion as user5

## Objective

Login to the Linux server as user5 and perform the following tasks:

1. Delete /dir1.
2. Delete /dir2.
3. Delete /dir3.
4. Delete /dir5.
5. Delete /dir7.
6. Delete /f1 and /f4.
7. Delete /opt/dir14.

## Step 1: Configure Sudo Access for user5

Login as root and add user5 to the wheel group:

```bash
usermod -aG wheel user5
```

Configure passwordless sudo access:

```bash
visudo -f /etc/sudoers.d/user5
```

Add the following line:

```text
user5 ALL=(ALL) NOPASSWD: ALL
```

Set the correct permissions:

```bash
chmod 440 /etc/sudoers.d/user5
```

Validate the sudoers configuration:

```bash
visudo -c
```

## Step 2: Login as user5

Switch to user5:

```bash
su - user5
```

Verify the current user:

```bash
whoami
```

Expected output:

```text
user5
```

Verify sudo access:

```bash
sudo whoami
```

Expected output:

```text
root
```

**Note:** If group membership was just changed, start a new login session for user5.

## Step 3: Delete Directory /dir1

Execute:

```bash
sudo rm -rf /dir1
```

Verify:

```bash
ls -ld /dir1
```

## Step 4: Delete Directory /dir2

Execute:

```bash
sudo rm -rf /dir2
```

Verify:

```bash
ls -ld /dir2
```

## Step 5: Delete Directory /dir3

Execute:

```bash
sudo rm -rf /dir3
```

Verify:

```bash
ls -ld /dir3
```

## Step 6: Delete Directory /dir5

Execute:

```bash
sudo rm -rf /dir5
```

Verify:

```bash
ls -ld /dir5
```

## Step 7: Delete Directory /dir7

Execute:

```bash
sudo rm -rf /dir7
```

Verify:

```bash
ls -ld /dir7
```

## Step 8: Delete Files /f1 and /f4

Delete both files using a single command:

```bash
sudo rm -f /f1 /f4
```

Verify:

```bash
ls -l /f1 /f4
```

Both files should report that they do not exist.

## Step 9: Delete Directory /opt/dir14

Execute:

```bash
sudo rm -rf /opt/dir14
```

Verify:

```bash
ls -ld /opt/dir14
```

The directory should no longer exist.

## Step 10: Final Verification

Run all verification commands:

```bash
ls -ld /dir1 /dir2 /dir3 /dir5 /dir7
ls -l /f1 /f4
ls -ld /opt/dir14
```

All specified paths should report that they do not exist.

## Step 11: Final Verification Checklist

- [ ] Added user5 to the wheel group.
- [ ] Configured passwordless sudo access.
- [ ] Validated sudoers configuration.
- [ ] Logged in as user5.
- [ ] Verified sudo privileges.
- [ ] Deleted /dir1.
- [ ] Deleted /dir2.
- [ ] Deleted /dir3.
- [ ] Deleted /dir5.
- [ ] Deleted /dir7.
- [ ] Deleted /f1.
- [ ] Deleted /f4.
- [ ] Deleted /opt/dir14.
- [ ] Verified all deletions.

## Useful Linux Commands Summary

| Command | Purpose |
|---|---|
| usermod -aG wheel user5 | Add user5 to wheel |
| visudo | Safely edit sudoers configuration |
| chmod 440 | Set sudoers file permissions |
| visudo -c | Validate sudoers configuration |
| su - user5 | Switch to user5 |
| sudo whoami | Verify sudo privileges |
| rm -rf | Delete directories recursively |
| rm -f | Delete files without prompting |
| ls -ld | Verify directory existence |
| ls -l | Verify file existence |

---

# Module 10: User, Group and Filesystem Cleanup as Root

## Objective

Login to the Linux server as root and perform the following tasks:

1. Delete users: user1, user2, user3, user4, user5.
2. Delete groups: app, aws, database, devops.
3. Delete home directories of user1, user2, user3, user4, user5 if they still exist.
4. Unmount the /data filesystem.
5. Delete the /data directory.

## Step 1: Login as Root

Switch to root:

```bash
sudo -i
```

Verify the current user:

```bash
whoami
```

Expected output:

```text
root
```

## Step 2: Delete Users and Their Home Directories

There are two ways to delete users and their home directories.

### Option 1: Delete User and Home Directory Separately

Delete the user account:

```bash
userdel user1
```

Remove the home directory:

```bash
rm -rf /home/user1
```

Repeat for the remaining users if using this method.

### Option 2: Delete Users and Home Directories Together (Recommended)

Delete user1:

```bash
userdel -r user1
```

Delete user2:

```bash
userdel -r user2
```

Delete user3:

```bash
userdel -r user3
```

Delete user4:

```bash
userdel -r user4
```

Delete user5:

```bash
userdel -r user5
```

**Explanation:**

- `userdel`: Deletes a user account.
- `-r`: Removes the user's home directory and mail spool along with the account.

### Verify User Deletion

Check user1:

```bash
id user1
```

Check user2:

```bash
id user2
```

Check user3:

```bash
id user3
```

Check user4:

```bash
id user4
```

Check user5:

```bash
id user5
```

List remaining home directories:

```bash
ls -l /home/
```

**Note:** If a user account has already been deleted but its home directory still exists, remove that directory separately after verifying its ownership and contents.

## Step 3: Delete Groups

Delete the app group:

```bash
groupdel app
```

Delete the aws group:

```bash
groupdel aws
```

Delete the database group:

```bash
groupdel database
```

Delete the devops group:

```bash
groupdel devops
```

### Verify Group Deletion

```bash
getent group app
getent group aws
getent group database
getent group devops
```

If the groups have been deleted successfully, these commands should return no matching entries.

**Note:** A group cannot normally be deleted while it is still the primary group of an existing user. Check for remaining users if groupdel reports an error.

## Step 4: Unmount the /data Filesystem

Before unmounting, check the mounted filesystems:

```bash
df -h
```

Check the /data mount:

```bash
findmnt /data
```

Unmount the filesystem:

```bash
umount /data
```

### Explanation

- `umount`: Disconnects a mounted filesystem from its mount point.
- `/data`: Mount directory of the EBS filesystem.

### Verify Unmounting

```bash
df -h
```

The /data filesystem should no longer appear.

You can also verify using:

```bash
lsblk -f
```

The EBS volume should no longer show /data as its mount point.

**Important:** If the filesystem is busy, check which processes are using /data before attempting to unmount it again.

## Step 5: Delete the /data Directory

After successfully unmounting the filesystem, remove the empty directory:

```bash
rmdir /data
```

### Explanation

- `rmdir`: Removes an empty directory.
- It will not remove a directory containing files or subdirectories.

### Verify Directory Deletion

```bash
ls -ld /data
```

The command should report that the directory does not exist.

## Step 6: Final Verification

### Verify User Deletion

```bash
id user1
id user2
id user3
id user4
id user5
```

### Verify Group Deletion

```bash
getent group app
getent group aws
getent group database
getent group devops
```

### Verify Home Directories

```bash
ls -l /home/
```

### Verify Filesystem Unmounting

```bash
df -h
lsblk -f
```

### Verify /data Directory Deletion

```bash
ls -ld /data
```

## Step 7: Final Verification Checklist

- [ ] Logged in as root.
- [ ] Deleted user1.
- [ ] Deleted user2.
- [ ] Deleted user3.
- [ ] Deleted user4.
- [ ] Deleted user5.
- [ ] Removed remaining home directories if any.
- [ ] Deleted app group.
- [ ] Deleted aws group.
- [ ] Deleted database group.
- [ ] Deleted devops group.
- [ ] Checked mounted filesystems.
- [ ] Unmounted /data.
- [ ] Verified EBS filesystem is no longer mounted.
- [ ] Deleted the empty /data directory.
- [ ] Verified all cleanup operations.

## Useful Linux Commands Summary

| Command | Purpose |
|---|---|
| sudo -i | Switch to root |
| whoami | Display current user |
| userdel | Delete a user account |
| userdel -r | Delete user and home directory |
| rm -rf | Remove directories and their contents |
| groupdel | Delete a group |
| getent group | Verify group information |
| id | Verify user information |
| df -h | Display filesystem utilization |
| findmnt | Display mount information |
| umount | Unmount a filesystem |
| lsblk -f | Display block device filesystem information |
| rmdir | Remove an empty directory |

---

# Module 11: Detach and Delete EBS Volume and Terminate EC2 Instance

## Objective

Login to the AWS Management Console and perform the following tasks:

1. Identify all EBS volumes attached to the EC2 instance.
2. Detach the additional EBS volume from the EC2 instance.
3. Delete the detached EBS volume.
4. Terminate the EC2 instance.

## Step 1: Login to AWS Management Console

1. Open the AWS Management Console.
2. Login using your AWS credentials.
3. Navigate to the EC2 service.

## Step 2: Identify Attached EBS Volumes

Navigate to:

```text
AWS Console → EC2 → Elastic Block Store → Volumes
```

1. Identify all EBS volumes attached to your EC2 instance.
2. Check the Instance ID associated with each volume.
3. Identify the additional volume created in Module 7.

Example:

| Property | Value |
|---|---|
| Volume Name | EBS-Additional-Volume |
| Volume Size | 5 GiB |
| Volume Type | gp3 |
| State | In-use |
| Attachment | Your EC2 instance |

**Important:** Make sure you identify the additional EBS volume and do not accidentally delete the root volume containing the operating system.

## Step 3: Detach the EBS Volume

1. Navigate to EC2 → Elastic Block Store → Volumes.
2. Select the additional EBS volume.
3. Click **Actions**.
4. Select **Detach volume**.
5. Confirm the detach operation if prompted.
6. Wait until the volume state changes from **In-use** to **Available**.

### Verify Detachment

Check the volume status:

```text
State: Available
```

The volume should no longer show an EC2 instance attachment.

## Step 4: Delete the EBS Volume

After successful detachment:

1. Select the detached EBS volume.
2. Click **Actions**.
3. Select **Delete volume**.
4. Confirm the deletion.

### Verify Volume Deletion

Navigate to:

```text
EC2 → Elastic Block Store → Volumes
```

Verify that the additional EBS volume is no longer listed.

**Note:** Deleting an EBS volume permanently deletes its stored data. Make sure you no longer need the data before proceeding.

## Step 5: Troubleshooting EBS Detachment

If you are unable to detach the EBS volume, check the following:

### 1. Check Whether the Filesystem Is Mounted

Login to the EC2 instance and run:

```bash
df -h
```

Check whether the EBS filesystem is mounted on /data.

### 2. Unmount the Filesystem

If the filesystem is still mounted:

```bash
sudo umount /data
```

Verify:

```bash
df -h
lsblk -f
```

### 3. Stop the EC2 Instance if Required

If detachment still fails:

1. Navigate to EC2 → Instances.
2. Select your EC2 instance.
3. Click **Instance state**.
4. Select **Stop instance**.
5. Wait until the instance state becomes **Stopped**.
6. Retry detaching the additional EBS volume.

**Important:** Root EBS volumes generally require the EC2 instance to be stopped before detachment. Do not detach the root volume if your intention is only to remove the additional EBS volume.

## Step 6: Terminate the EC2 Instance

After deleting the additional EBS volume:

1. Navigate to EC2 → Instances.
2. Select the EC2 instance you created for this Linux assessment.
3. Verify that you have selected the correct instance.
4. Click **Instance state**.
5. Select **Terminate instance**.
6. Confirm the termination operation.

### Verify Instance Termination

Wait until the instance state changes to:

```text
Terminated
```

The EC2 instance should no longer be running.

## Step 7: Final Verification

### Verify EBS Volume Deletion

Navigate to:

```text
EC2 → Elastic Block Store → Volumes
```

Confirm that EBS-Additional-Volume has been deleted.

### Verify EC2 Instance Termination

Navigate to:

```text
EC2 → Instances
```

Confirm that the instance state is:

```text
Terminated
```

## Step 8: Final Verification Checklist

- [ ] Logged in to AWS Management Console.
- [ ] Opened the EC2 service.
- [ ] Identified all attached EBS volumes.
- [ ] Identified the additional 5 GiB EBS volume.
- [ ] Confirmed that the filesystem was unmounted.
- [ ] Detached the additional EBS volume.
- [ ] Verified the volume state changed to Available.
- [ ] Deleted the additional EBS volume.
- [ ] Verified the volume was deleted.
- [ ] Selected the correct EC2 instance.
- [ ] Terminated the EC2 instance.
- [ ] Verified the instance state is Terminated.

## Important AWS Notes

- Always identify the correct EBS volume before detaching or deleting it.
- Unmount filesystems before detaching their EBS volumes.
- Deleting an EBS volume permanently removes its data.
- Terminating an EC2 instance may also delete its root EBS volume if Delete on termination is enabled.
- Additional EBS volumes are not necessarily deleted when an EC2 instance is terminated.
- Check for other EBS volumes, snapshots, Elastic IP addresses and other billable resources that may remain after termination.

---
