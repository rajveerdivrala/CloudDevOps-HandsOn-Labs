Task-019: Launch EC2 Windows Server 2025 Instance and connect via RDP — screenshot the desktop:

1. Log in to the AWS Management Console

Open the AWS Console in your browser and log in. In the top search bar, type "EC2" and click on the EC2 Dashboard.
Verification: You should see the main EC2 Dashboard page load successfully.


2. Launch Instance

On the EC2 Dashboard, click the orange "Launch instance" button.
Verification: The launch instance configuration page will appear on your screen.


3. Configure Instance Details

Name: Give your instance a name (e.g., Windows-Server-2025-Demo).
Application and OS Images (AMI): Search for Windows Server 2025 in the search box and select the Microsoft Windows Server 2025 (Base or Full version) AMI.
Instance Type: Choose a free tier eligible instance type or one matching your requirements (e.g., t3.medium or recommended specs).
Key Pair: Select an existing key pair from the dropdown, or click "Create new key pair" to download a .pem or .ppk file (this will be used to decrypt your Windows password).
Verification: All basic settings fields should be filled out correctly with your desired options.


4. Configure Network and Firewall (Security Group) Settings

Network Settings: Leave the default VPC and Subnet as they are.
Firewall (Security Group): Check the box for "Allow RDP traffic from". This opens port 3389, which allows you to connect via Remote Desktop (RDP). Set the source IP to My IP or Anywhere (0.0.0.0/0).
Verification: The security group rule allowing TCP port 3389 should be visible under network settings.


5. Launch the Instance

Review your settings and click the "Launch instance" button on the bottom right. Once successful, click "View all instances".
Verification: Your instance state will change from "Pending" to "Running".

<img width="752" height="347" alt="task_19_a" src="https://github.com/user-attachments/assets/1dad4aef-3bd2-4489-9cae-3ef6d84657e0" />



6. Generate the Windows Server Password

Select your running instance, click the Connect button at the top, and go to the RDP client tab. Click "Get password", upload your downloaded .pem private key file, and click "Decrypt Password". Copy and securely save the decrypted Administrator password.
Verification: The plain-text Administrator password will be successfully displayed on the screen.

<img width="953" height="332" alt="task_19_b" src="https://github.com/user-attachments/assets/7cfa0670-76e3-4b39-95d5-35a4e0510abc" />


7. Connect via RDP

On the same RDP client page, click "Download remote desktop file" (.rdp). Double-click the downloaded file, click Connect on the warning popup, and enter Administrator as the username and the decrypted password. Accept any certificate warnings by clicking Yes or Continue.

<img width="940" height="401" alt="task_19_c" src="https://github.com/user-attachments/assets/c85ce03a-8b7c-4257-a805-144f315de120" />

<img width="371" height="361" alt="task_19_d" src="https://github.com/user-attachments/assets/42b7962e-1dec-462f-80bf-64682cd9d073" />

Verification: The Windows Server 2025 desktop screen will open, confirming a successful connection.

Post Configuration Setup: Successfully provisioned a custom directory structure (Rajveer/Windows-Server-2025-File) directly on the newly deployed Windows Server 2025 GUI environment to organize working files.

<img width="4032" height="3024" alt="IMG_0214" src="https://github.com/user-attachments/assets/329f9eb1-2299-4fc5-8df8-8801c2b649f1" />

<img width="4032" height="3024" alt="IMG_0216" src="https://github.com/user-attachments/assets/cbbe8c63-3a0b-4061-bcf0-5618f0447bcd" />





