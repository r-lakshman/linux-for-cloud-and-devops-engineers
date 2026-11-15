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
  ```bash
  ec2-user
  ```
- `hostname`
  ```bash
  hostname
  ```
- `pwd`
  ```bash
  /home/ec2-user
  ```
- `df -h`
  ```bash
  df -h
  ```

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






