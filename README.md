# 🔐 Password Cracking on Open-Source System (Metasploitable2)

## 📌 Project Objective
Recover user passwords from an open-source system using password-cracking tools like **John the Ripper**, or **Hashcat**. The goal is to demonstrate techniques used in password recovery and understand system vulnerabilities while learning how to mitigate these threats effectively.

---

## 🛠️ Tools & Technologies Used

| Tool | Purpose |
|------|---------|
| **Metasploitable2** | Vulnerable virtual machine used as the target system |
| **John the Ripper** | Password hash cracking tool |
| **Hashcat** (optional) | GPU-based password recovery tool |
| **rockyou.txt** | Common password wordlist used for dictionary attacks |
| **Kali Linux** | Attacker machine with pre-installed security tools |

---

## 🧪 Environment Setup

- **Host System**: VirtualBox or VMware installed on local machine
- - **Attacker**: Kali Linux

    ![Kali Linux](images/kali.png)

- **Target**: Metasploitable2 VM

  ![Meta](images/metasploitable2.png)

- **Network Mode**: Host-only or Internal to simulate a secure lab

---

## 🔄 Methodology (Step-by-Step)

### Step 1: Extract Password Hashes

- Gain access to the `/etc/shadow` and `/etc/passwd` files on the target (e.g., via a known exploit or misconfigured privilege)
- Combine both files using:
  ```bash
  unshadow /etc/passwd /etc/shadow > combined.txt
  ```

### Step 2: Crack Passwords Using John the Ripper

- Run John with a wordlist:
  ```bash
  john --wordlist=/usr/share/wordlists/rockyou.txt --format=sha512crypt combined.txt
  ```
- View cracked passwords:
  ```bash
  john --show combined.txt
  ```
---

## 🧩 Challenges Faced and Solutions
During the execution of this project, I encountered a few key challenges that helped deepen my understanding of real-world troubleshooting in cybersecurity environments:

### 1. Black Screen Issue on Metasploitable2 Boot
Upon starting the Metasploitable2 VM, the screen remained blank. This was resolved by switching the VM's graphics settings to use "Accelerate 3D graphics" or rebooting the VM cleanly from VMware’s interface.

### 2. SSH Connection Error – No Matching Host Key Type
While trying to SSH from Kali to Metasploitable2, an error occurred due to deprecated host key algorithms. I resolved it by creating a custom SSH config file that explicitly allowed the older key types ssh-rsa and ssh-dss.

### 3. Permission Denied on Shadow File Copy
Attempting to SCP the shadow file resulted in a permission error. Since elevated permissions are required to access /etc/shadow, I used sudo cp within the VM to copy it to a readable location (/home/msfadmin/) before transferring.

### 4. John the Ripper: “No password hashes loaded” Error
When attempting to run John the Ripper, I got “No password hashes loaded (see FAQ)”. The combined password and shadow file was not properly formatted. I used the unshadow tool to merge both files correctly. This then generated a valid input file for hash cracking.

### 5. Wordlist Not Found
When using John the Ripper with the rockyou.txt wordlist, an error indicated the file was missing. I corrected this by extracting the compressed version using:
`sudo gzip -d /usr/share/wordlists/rockyou.txt.gz`

### 6. Partial Hash Cracking
Out of 7 password hashes, only 3 were cracked using rockyou.txt. I documented the success and identified remaining hashes for future cracking using additional wordlists, mangling rules, or more powerful tools like Hashcat.

These obstacles not only tested my patience and problem-solving skills but also reinforced the importance of adaptable troubleshooting in cybersecurity workflows.

---

## Results
Using John the Ripper with the `rockyou.txt` wordlist, I successfully recovered 3 plaintext passwords from the combined `passwd` and `shadow` file hashes.

| Username | Password |
|------|---------|
| **Sys** | batman |
| **Klog** | 123456789 |
| **Service** | service |

---

## 📸 Screenshots

> - Cracking process in terminal

![Cracking](images/password-cracking.png)

![Cracking](images/password-cracking2.png)

> - Successfully recovered passwords

![Cracked](images/cracked-hashes.png)

---

## 🛡️ Mitigation Recommendations

To reduce the risk of password cracking:

- Use **complex, lengthy passwords** (avoid dictionary words)
- Enforce **password rotation**, **rate limiting** and **account lockouts** after failed login attempts
- Apply **salting and strong hash algorithm** (e.g., bcrypt, scrypt) before storing passwords
- Implement **Multi-Factor Authentication (MFA)**
- Conduct regular **security audits** and **user education**
- Monitor logs for **brute-force activity**

---

## ✅ Lessons Learned

- Hands-on exposure to real-world password cracking techniques
- Importance of password complexity and system hardening
- Understanding how adversaries exploit weak security configurations

---

Date: 17th April, 2025.
