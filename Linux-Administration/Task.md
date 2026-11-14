Linux System Administration & AWS Infrastructure Management
Pre-Requisites
Login to AWS Cloud.
Create a Linux-based Amazon EC2 instance to complete the assignment.
Deployment
1. Login to the server as root and perform the following
Create users and set passwords: user1, user2, user3.
Create groups: devops, aws.
Change the primary group of user2 and user3 to devops.
Add the aws group as a secondary group to user1.
Create the file and directory structure shown in the provided diagram.
Change the group ownership of /dir1, /dir7/dir10, and /f2 to devops.
Change the ownership of /dir1, /dir7/dir10, and /f2 to user1.
2. Login as user1 and perform the following
Create users and set passwords: user4, user5.
Create groups: app, database.
3. Login as user4 and perform the following
Create directory: /dir6/dir4.
Create file: /f3.
Move the file from /dir1/f1 to /dir2/dir1/dir2.
Rename the file /f2 to /f4.
4. Login as user1 and perform the following
Create directory: /home/user2/dir1.
Change to /dir2/dir1/dir2/dir10 and create the file /opt/dir14/dir10/f1 using a relative path.
Move /opt/dir14/dir10/f1 to the user1 home directory.
Delete the directory /dir4 recursively.
Delete all child files and directories under /opt/dir14 using a single command.
Write the following text to /f3 and save it:

Linux assessment for an DevOps Engineer!! Learn with Fun!!

5. Login as user2 and perform the following
Create file: /dir1/f2.
Delete /dir6.
Delete /dir8.
Replace DevOps with devops in /f3 without using an editor.
Using Vi Editor, copy line 1 and paste it 10 times in /f3.
Search for the pattern Engineer and replace it with engineer using a single command.
Delete /f3.
6. Login as root and perform the following
Search for files named f3 in the server and list all absolute paths where the files are found.
Show the count of files in the / directory.
Print the last line of /etc/passwd.
7. AWS EBS Volume Creation and Attachment
Login to AWS.
Create a 5 GB EBS volume in the same Availability Zone as the EC2 instance.
Attach the EBS volume to the EC2 instance.
8. Login as root and perform the following
Create a filesystem on the newly attached EBS volume.
Mount the filesystem on the /data directory.
Verify filesystem utilization using df -h. The output must show the /data filesystem.
Create file /data/f1.
9. Login as user5 and perform the following
Delete /dir1.
Delete /dir2.
Delete /dir3.
Delete /dir5.
Delete /dir7.
Delete /f1 and /f4.
Delete /opt/dir14.
10. Login as root and perform the following
Delete users: user1, user2, user3, user4, user5.
Delete groups: app, aws, database, devops.
Delete the home directories of all users (user1, user2, user3, user4, user5) if they still exist.
Unmount the /data filesystem.
Delete the /data directory.
11. AWS Resource Cleanup
Login to AWS.
Detach the EBS volume from the EC2 instance.
Delete the EBS volume.
Terminate the EC2 instance.
Assignment Completion

Verify that all the above Linux administration and AWS infrastructure management tasks have been completed successfully.
