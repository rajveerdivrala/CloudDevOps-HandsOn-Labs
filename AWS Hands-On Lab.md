# AWS Hands-On Lab: Application Load Balancer, EC2 Instances & Troubleshooting Guide

This document captures my practical hands-on learning, architectural concepts, and troubleshooting steps while setting up an AWS Application Load Balancer (ALB) across multiple Linux EC2 instances, managing Elastic IPs, and resolving instance reboot scenarios.

---

## 1. Initial Lab Setup & Architecture Overview

* **Multi-VM Provisioning:** Created two independent Linux EC2 virtual machine instances in AWS.
* **Remote Access:** Connected securely to both instances via SSH using **MobaXterm**.
* **Web Server Installation:** Installed the Nginx web server on both nodes (`sudo apt install nginx`) and started the service (`sudo systemctl start nginx`).
* **Content Customization:** 
  * **Server 1:** Retained the default `"Welcome to Nginx"` landing page.
  * **Server 2:** Customized the HTML response text using an `echo` command to display: `"I am Cloud DevOps Hub Batch 45 student"`.
* **Load Balancer Integration:** Provisioned an Application Load Balancer (ALB) via the AWS Console, selected the appropriate Availability Zones, attached security groups, and registered both backend VMs to the target group.
* **Traffic Validation:** Accessed the Application Load Balancer via its DNS name in a web browser. Verified round-robin traffic splitting across both backend servers upon successive browser refreshes.

---

## 2. Core Architecture Learnings: Elastic IP vs. Load Balancer

* **What is an Elastic IP?** An Elastic IP provides a static, permanent public IPv4 address assigned to a *single* standalone EC2 instance. If attached, the instance retains its public IP even after a restart.
* **Why Load Balancers Don't Need Backend Elastic IPs:** 
  * The Application Load Balancer routes incoming client traffic to backend instances using their **Private IP addresses** within the VPC.
  * Therefore, individual backend VMs sitting behind an ALB do not require public or Elastic IPs. High availability and fault tolerance are managed directly by the ALB and its target group health checks.

---

## 3. Troubleshooting Scenario: VM Restart & Traffic Recovery

### **The Problem (Symptom)**
After stopping and restarting one of the backend EC2 instances (which did not have an Elastic IP assigned), traffic stopped routing to that specific instance through the Load Balancer DNS. Browser refreshes only displayed responses from the other server, and the restarted instance showed an **"out of service"** (unhealthy) health status in the AWS Target Group.

### **Root Cause**
When an EC2 instance without an Elastic IP is stopped and started, AWS assigns it a brand-new dynamic Public IP address. Consequently, the temporary network mapping desynchronized momentarily, and the Nginx service on the restarted instance required verification and service checking.

---

## 4. Step-by-Step Troubleshooting Guide

<steps>
  <step title="Reconnected to the Restarted VM" subtitle="MobaXterm & AWS Console">
    Checked the updated public IPv4 address of the restarted instance from the AWS EC2 console and opened a fresh SSH session in MobaXterm using the instance's `.pem` private key.
  </step>
  <step title="Verified and Started Nginx Service" subtitle="Linux Terminal">
    Checked the service status in the terminal:
    ```bash
    sudo systemctl status nginx
    ```
    If stopped or inactive, started the Nginx service manually:
    ```bash
    sudo systemctl start nginx
    ```
  </step>
  <step title="Monitored Target Group Health Check" subtitle="AWS Console">
    Navigated to **Target Groups** under the EC2 dashboard, inspected the targets tab, and allowed 1–3 minutes for the ALB health checks to probe the instance. Verified that the status automatically transitioned from `out of service` to **`healthy`**.
  </step>
  <step title="Final Traffic Validation" subtitle="Browser Verification">
    Refreshed the Load Balancer DNS URL in the browser and confirmed that incoming traffic successfully alternated between both backend servers once again.
  </step>
</steps>

---

## 5. Key Takeaways & Best Practices
* **Load Balancers** abstract backend infrastructure and eliminate the dependency on static public IPs for individual application servers.
* **Private IP Routing** inside a VPC ensures secure and reliable communication between the ALB and target instances.
* Always verify service status (`systemctl status`) and target health metrics after performing instance reboots in a cloud environment.
