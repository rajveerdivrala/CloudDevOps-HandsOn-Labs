Task 017: Solve 5 real Linux production scenarios from the session — document your complete solution:


>> Scenari01: Securing sensitive file.

A file created in /tmp/task017 
file name is scenario
checked the permissions with ls -l

-rw-r--r-- 1 root root 22 Oct  4 08:44 scenario

suppose the file has very sensitive data and it should not be readable anyone except owner.

I will apply the 600 pemission code to make this unreadable for group and others.

root@new:/tmp/task017# chmod 600 scenario
root@new:/tmp/task017# ls -l
total 4
-rw------- 1 root root 22 Oct  4 08:44 scenario

<img width="381" height="146" alt="Scenario1" src="https://github.com/user-attachments/assets/e50d3a4f-aaec-4b0e-be7b-68cbe83db455" />


>> Scenario2 Changes the ownership of a file

Created a file :scenario1
Need to change the ownership 
first created a user :ravi
and changed the ownership wtih cmd chown root:ravi scenario1

root@new:/tmp/task017# chown root:ravi scenario1
root@new:/tmp/task017# ls -l
total 8
-rw------- 1 root root 22 Oct  4 08:44 scenario
-rw-r--r-- 1 root ravi 34 Oct  4 08:53 scenario1
<img width="482" height="416" alt="Scenario2" src="https://github.com/user-attachments/assets/c73cc139-1ea3-4b22-a6f9-3762e6bec312" />


>> Scenario3 Jenkins is installed and running on a cloud virtual machine, but developers cannot access the web dashboard via the VM's external IP on port 8080.

first of all installed Jenkins , , installing java as Jenkins required java, adding the source repository of jenkins in source list and updating the packages and installing Jenkins with the cmds: apt-get update, apt install Jenkins

<img width="671" height="233" alt="Scenario3_a" src="https://github.com/user-attachments/assets/3917332f-4222-455f-96c4-cc3ef15312b3" />

<img width="612" height="73" alt="Scenario3_b" src="https://github.com/user-attachments/assets/aab4b29a-71d0-4c28-9fca-651b076a8a9f" />

<img width="653" height="161" alt="Scenrario3_c" src="https://github.com/user-attachments/assets/508786cf-631f-447e-b8c9-9bd7cb83cc7a" />

Still Jenkins is not accessible with external ip:8080

By default, Jenkins is configured by its developers to run its internal web server on Port 8080.

to solve this issue we need to create a new firewall rule with port 8080

to add a new firewall rule we use these stps :
- search in Google cloud console vpc network> firewall
- create firewall rule
- name: allow-jenkins-8080
- Targets: All instances in the network
- Source IP ranges: 0.0.0.0/0 
- Protocols and ports: check TCP and enter 8080 in the box
- click create


Now we will search externalIP:8080 and we see the Jenkins is working.

<img width="959" height="510" alt="Scenario3_d" src="https://github.com/user-attachments/assets/60a58902-0c15-46ac-a36c-81e9b62ccd20" />



>>Scenario4: Application logs are growing rapidly on a production server, causing performance degradation and storage alerts. Administrators need to inspect disk utilization and block devices.

to check all available storage block devices and partitions:

<lsblk>

Check file system disk space utilization in a human-readable format:

<df -h>

<img width="361" height="140" alt="Scenario4" src="https://github.com/user-attachments/assets/0cd0c40e-ea78-4e6f-bfe0-eb9194a7b5b5" />


>>Scenario 5: Provisioning a Secure Non-Root Service User

A new background automation tool needs to be deployed securely under an isolated non-privileged user account rather than root to follow the principle of least privilege.

Create the new user account (requires root or sudo privileges):
adduser raj

Switch to the newly created user session to test access:
su raj

<img width="519" height="310" alt="Scenario5" src="https://github.com/user-attachments/assets/77653b92-81b3-4c8d-bce7-2ed75c1f3387" />


>>Scenario6: Check the status of system service jenkins:

The Situation:Jenkins fails to start automatically after a system reboot, causing application downtime.

to check the current status of the failing service:
use the cmd <systemctl status jenkins>

or to inspect detailed system journal error logs for that specific service:
journalctl -u jenkins -e

restarting the service:
systemctl restart Jenkins

<img width="662" height="263" alt="Scenario6" src="https://github.com/user-attachments/assets/96c9b59a-f91e-4ddf-886f-3dee9f7337dc" />




Task-018: Write a system health-check bash script checking CPU RAM Disk and running Services

created a script file bashscript.sh in vi editor. added this content:
"""
#!/bin/bash
echo "***Ram Memmory And Swap Space***"
free -h

echo "***Here we can check which processes is consuming most CPU and RAM***"
top


echo "***Here we can check the disk usages.***"
df -h


echo "Here we can see current running processes and service***"
ps


echo "***Here we can check current folder's Size.***"
du


echo "Here we can check CPU architecture, cores, model and capability***"
lscpu
"""

use the cmd <./bashscript.sh> **To open a shell script we use <./>

Got the error "permission denied".

<img width="383" height="81" alt="Task18" src="https://github.com/user-attachments/assets/ceaa460a-4513-4208-ad41-115cfc046120" />

I checked the permissions of this file with ls -l cmd and found that there is no executable permission for this file:
-rw-r--r-- 1 root root 411 Oct  4 09:55 bashscript.sh
I used <chmod 744 bashscript> and provided all permissions including executable to root user.
root@new:/tmp/task017/scenario6# chmod 744 bashscript.sh
root@new:/tmp/task017/scenario6# ls -l
total 4
-rwxr--r-- 1 root root 411 Oct  4 09:55 bashscript.sh
root@new:/tmp/task017/scenario6# 

<img width="427" height="121" alt="Task18_a" src="https://github.com/user-attachments/assets/c396f2f9-a64c-43f8-9912-962aaf44c7a2" />

<img width="435" height="74" alt="Task18_b" src="https://github.com/user-attachments/assets/cb926cd9-d8e2-4ccc-a8bd-ba48b630d527" />


execute the monitoring script: ./bashscript.sh

It is working.

<img width="623" height="305" alt="Task18_c" src="https://github.com/user-attachments/assets/6a0deea6-c6eb-4117-9157-6e27eb16b9c4" />

