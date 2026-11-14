# Linux System Administration & AWS Infrastructure Management - Lab Assignment

## Pre-Requisites
- Login to the AWS Cloud console.
- Create a Linux-based Amazon EC2 instance to complete the assignment.

---

## Deployment Tasks

### 1. Root User – Initial Configuration
- Create users and set passwords: `user1`, `user2`, `user3`.
- Create groups: `devops`, `aws`.
- Change the primary group of `user2` and `user3` to `devops`.
- Add the `aws` group as a secondary group to `user1`.
- Create the file and directory structure shown in the provided diagram.
- Change the group ownership of `/dir1`, `/dir7/dir10`, and `/f2` to `devops`.
- Change the ownership of `/dir1`, `/dir7/dir10`, and `/f2` to `user1`.

### 2. User1 – User and Group Management
- Create users and set passwords: `user4`, `user5`.
- Create groups: `app`, `database`.

### 3. User4 – File and Directory Management
- Create directory: `/dir6/dir4`.
- Create file: `/f3`.
- Move `/dir1/f1` to `/dir2/dir1/dir2`.
- Rename `/f2` to `/f4`.

### 4. User1 – Advanced File and Directory Operations
- Create directory: `/home/user2/dir1`.
- Navigate to `/dir2/dir1/dir2/dir10` and create `/opt/dir14/dir10/f1` using a relative path.
- Move `/opt/dir14/dir10/f1` to the `user1` home directory.
- Delete `/dir4` recursively.
- Delete all child files and directories under `/opt/dir14` using a single command.
- Write the following text to `/f3`: `Linux assessment for an DevOps Engineer!! Learn with Fun!!`

### 5. User2 – File Operations and Text Manipulation
- Create file: `/dir1/f2`.
- Delete `/dir6`.
- Delete `/dir8`.
- Replace `DevOps` with `devops` in `/f3` without using an editor.
- Using Vi Editor, copy line 1 and paste it 10 times in `/f3`.
- Search for `Engineer` and replace it with `engineer` using a single command.
- Delete `/f3`.

### 6. Root User – File Search and System Information
- Search for files named `f3` and list all absolute paths where they are found.
- Display the count of regular files directly under `/`.
- Print the last line of `/etc/passwd`.

---

## AWS EBS Volume Creation and Attachment

### 7. AWS – EBS Volume Management
- Create a **5 GB EBS volume** in the same Availability Zone as the EC2 instance.
- Attach the EBS volume to the EC2 instance.

### 8. Root User – Filesystem Management
- Create a filesystem on the newly attached EBS volume.
- Mount the filesystem on `/data`.
- Verify filesystem utilization using `df -h`. The output must show the `/data` filesystem.
- Create file `/data/f1`.

---

## Cleanup Tasks

### 9. User5 – File and Directory Cleanup
- Delete `/dir1`, `/dir2`, `/dir3`, `/dir5`, and `/dir7`.
- Delete `/f1` and `/f4`.
- Delete `/opt/dir14`.

### 10. Root User – System Cleanup
- Delete users: `user1`, `user2`, `user3`, `user4`, `user5`.
- Delete groups: `app`, `aws`, `database`, `devops`.
- Delete the home directories of all five users if they still exist.
- Unmount the `/data` filesystem.
- Delete the `/data` directory.

### 11. AWS – EBS and EC2 Cleanup
- Detach the EBS volume from the EC2 instance.
- Delete the EBS volume.
- Terminate the EC2 instance.

---

## Assignment Completion
- Verify that all Linux administration and AWS infrastructure management tasks have been completed successfully.
