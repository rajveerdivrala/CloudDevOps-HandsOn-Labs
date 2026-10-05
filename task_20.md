🔨 Task-020: Configure Apache or Nginx on Linux EC2 with MobaXterm — test via browser with public IP


# Deploying and Troubleshooting Nginx Web Server on Amazon Linux EC2

### 1. Launching the Amazon Linux EC2 Instance

* Open the AWS Management Console and navigate to the **EC2 Dashboard**.
* Click on **Launch instance**.
* Provide a descriptive name for your instance.
* Under **Application and OS Images (AMI)**, select **Amazon Linux** (such as Amazon Linux 2023).
* Choose an instance type (e.g., `t3.micro` or free-tier eligible).
* Select or create a key pair (`.pem` file) for SSH access.
* Review the configuration and click **Launch instance**.
*
* <img width="959" height="479" alt="task_20_a" src="https://github.com/user-attachments/assets/572b903e-fe0b-4f79-814b-ae298663e951" />


### 2. Connecting via MobaXterm using SSH

* Copy the **Public IPv4 address** of your running EC2 instance from the AWS console.
* Open **MobaXterm**, click on **Session**, and choose **SSH**.
* Enter the Public IP in **Remote host**.
* Specify the default username as `ec2-user`.
* Check **Use private key** and attach your downloaded `.pem` private key file.
* Click **OK** to establish the terminal session and log into the server.

<img width="959" height="539" alt="task_20_b" src="https://github.com/user-attachments/assets/465162d4-80e4-41e0-a610-c0e7ca62c961" />

<img width="959" height="539" alt="task_20_c" src="https://github.com/user-attachments/assets/c53c0d8f-b955-4851-89b1-bf38c139b53c" />



### 3. Updating System Packages and Installing Nginx

* Run the package manager update command to ensure all system packages are up to date:
`sudo yum update -y`
* Install the Nginx web server package:
`sudo yum install nginx -y`

<img width="652" height="390" alt="task_20_e" src="https://github.com/user-attachments/assets/507a83ba-cb98-4d77-b7c2-2db7ca8a0301" />

<img width="756" height="386" alt="task_20_f" src="https://github.com/user-attachments/assets/934ba169-18ff-4c4c-8723-d14e37a28d83" />



### 4. Starting the Nginx Web Service

* Start the Nginx service so it begins serving requests:
`sudo systemctl start nginx`

### 5. Troubleshooting and Resolving the Browser Connection Issue

* **The Problem:** When accessing the instance's public IP in the web browser, the Nginx welcome page did not load (even after switching from HTTPS to HTTP).
* **The Root Cause:** The instance's Security Group (Firewall) did not have an inbound rule allowing HTTP traffic on Port 80, blocking external browser requests.
* **The Fix (Adding Firewall Rule):**
* Go to the **EC2 Dashboard** -> **Instances** -> Select your instance -> Click on the **Security** tab.
* Click on your **Security group** link, then click **Edit inbound rules**.
* Click **Add rule**, set **Type** to **HTTP** (Port 80), and set **Source** to **Anywhere-IPv4** (`0.0.0.0/0`).
* Click **Save rules**.



### 6. Final Verification

* Open your web browser, paste your EC2 instance's Public IP address using `http://<your-public-ip>`, and verify that the default Nginx welcome page loads successfully.

<img width="953" height="508" alt="task_20_g" src="https://github.com/user-attachments/assets/bb74d1f8-3d43-45db-a22a-a42b5e4712b3" />
