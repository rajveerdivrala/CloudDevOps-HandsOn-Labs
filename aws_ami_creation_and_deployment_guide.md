# AWS AMI Creation and Deployment Guide

This guide documents the practical steps for creating a custom Amazon Machine Image (AMI) from an existing EC2 instance and launching a new instance using that custom image.

---

## Part 1: Creating a Custom AMI

To capture the current state, configuration, and files of an existing EC2 instance into a reusable template, follow these steps:

1. Open the **Amazon EC2 Console**.
2. Navigate to **Instances** and select the source instance you want to image.
3. Click on **Actions** > **Image and templates** > **Create image**.
4. Configure the image details:
   * **Image name:** `raj_windows_AMI`
   * **Image description:** (Optional) Add a brief note about the image contents.
5. Click **Create image**.

* **Result:** AWS initiates the image creation process in the background (taking EBS snapshots). Once complete, you will receive your unique AMI ID.
* **Created AMI ID:** `ami-0abd3ffdeb666e76f`

---

## Part 2: Launching a New Instance from the Custom AMI

To spin up a brand new EC2 instance using your newly created custom image:

1. In the EC2 Console, click on **Launch instance**.
2. Under the **Application and OS Images (AMI)** section, choose **My AMIs** (specifically looking under *Owned by me* or *Custom AMIs*).
3. Select your custom image (`raj_windows_AMI` / `ami-0abd3ffdeb666e76f`).
4. Configure your instance settings:
   * Choose your desired **Instance Type** (e.g., `t2.micro` or `t3.micro`).
   * Select or create your **Key Pair** (`.pem` or `.ppk`).
   * Configure **Network Settings** to allow inbound traffic (e.g., check **Allow HTTP traffic from the internet** and **Allow HTTPS traffic from the internet**).
5. Click **Launch instance**.

* **Result:** AWS provisions a new virtual machine pre-configured with the exact state captured in your custom AMI.
* **Created Instance ID:** `i-0c3e19ab552e9ff17`

---

## Best Practices
* Always secure your private key pair immediately upon creation to avoid access lockout issues.
* Clean up or deregister outdated AMIs and snapshots to manage AWS storage costs effectively.