# 🛡️ OverTheWire Bandit: Level 16 ➔ Level 17 Walkthrough

A complete, step-by-step documentation of how to retrieve the RSA Private Key from Bandit Level 16, troubleshoot connection hurdles, transfer the key via SCP, and successfully authenticate into Bandit Level 17.

---

## 📋 Table of Contents
1. [Level Goal](#1-level-goal)
2. [Step 1: Port Scanning](#2-step-1-port-scanning)
3. [Step 2: Testing SSL Ports & Debugging](#3-step-2-testing-ssl-ports--debugging)
4. [Step 3: Saving and Securing the Private Key](#4-step-3-saving-and-securing-the-private-key)
5. [Step 4: Transferring the Key via SCP](#5-step-4-transferring-the-key-via-scp)
6. [Step 5: Logging into Bandit Level 17](#6-step-5-logging-into-bandit-level-17)

---

## 1. Level Goal
The goal of **Bandit Level 16** is to submit the current level's password to a specific port within the range **31000–32000**. Upon submission, instead of a standard plain text password, the server responds with an **RSA Private Key** which serves as the authentication credential for **Bandit Level 17**.

---

## 2. Step 1: Port Scanning
To find out which ports are open within the given range on the local machine, we use `nmap`.

* **Command:**
  ```bash
  nmap -sV -p 31000-32000 bandit
  ```

* **Result:** 
  The scan reveals several open ports, notably two running SSL/TLS services:
  * `31518` (`ssl/echo`)
  * `31790` (`ssl/unknown`)

---

## 3. Step 2: Testing SSL Ports & Debugging

### ❌ Attempt 1: Port 31518 (The Echo Trap)
If you connect to port `31518` using OpenSSL and pass your password:
```bash
echo "YOUR_CURRENT_PASSWORD" | openssl s_client -connect localhost:31518
```
* **Outcome:** This port behaves as an echo server. It simply bounces your input back to you and terminates the session (`KEYUPDATE` state). It is **not** the correct destination.

### ✅ Attempt 2: Port 31790 (The Target Port)
Port `31790` is the actual service handling the private key distribution. However, piping `echo` directly to OpenSSL without special flags often causes the connection to hang because the EOF (End-Of-File) signal cuts off the response prematurely.

* **The Solution:** Use the `-ign_eof` flag to keep the connection alive after sending the input, allowing the server to transmit the private key back to your screen.
* **Correct Command:**
  ```bash
  echo "YOUR_CURRENT_PASSWORD" | openssl s_client -connect localhost:31790 -ign_eof
  ```
* **Outcome:** The terminal outputs the full **RSA Private Key** block starting with `-----BEGIN RSA PRIVATE KEY-----`.

---

## 4. Step 3: Saving and Securing the Private Key
1. Copy the RSA Private Key block generated from port `31790`.
2. Create a temporary file inside the `/tmp` directory using `nano`:
   ```bash
   nano /tmp/key.pair
   ```
3. Paste the key inside, save it (`Ctrl + O`, `Enter`), and exit (`Ctrl + X`).
4. Set strict file permissions (SSH requires private keys to be readable only by the owner):
   ```bash
   chmod 600 /tmp/key.pair
   ```

---

## 5. Step 4: Transferring the Key via SCP
Direct SSH connections from inside a remote Bandit session back to `localhost` are blocked (`Connection from/to localhost is blocked`). Therefore, you must pull the key file to your local machine.

* **Command (Run from your **local machine/MobaXterm terminal**, not inside the remote SSH session):**
  ```bash
  scp -P 2220 bandit16@bandit.labs.overthewire.org:/tmp/path_to_your_temp_folder/key.pair ./bandit17key
  ```
* **Key Details:**
  * `-P 2220`: Specifies the remote SSH port using a capital `P`.
  * Remote path: Point to the exact directory inside `/tmp` where your key was saved.
  * `./bandit17key`: Saves the file into your local working directory.

---

## 6. Step 5: Logging into Bandit Level 17
Once the private key file (`bandit17key`) resides safely on your local client machine, authenticate into the next level using the `-i` identity flag.

* **Command:**
  ```bash
  ssh -i bandit17key bandit17@bandit.labs.overthewire.org -p 2220
  ```

* **Success:** You will bypass password authentication and land directly into **Bandit Level 17**! 🎉