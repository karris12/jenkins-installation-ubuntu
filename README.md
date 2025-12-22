

# How to Install Jenkins on Ubuntu 22.04

**Date:** Thursday, June 5, 2025
**OS:** Ubuntu 22.04 LTS

---

## 1. Change Hostname (Optional but Recommended)

```bash
sudo hostnamectl set-hostname Jenkins
```

---

## 2. Update System Packages

```bash
sudo apt update
```

---

## 3. Install Java 17 (Required for Jenkins)

```bash
sudo apt install openjdk-17-jdk -y
```

### Verify Java Installation

```bash
java -version
```

---

## 4. Install Maven (Optional – for Java Builds)

Maven is a popular build tool used for building Java applications.

```bash
sudo apt install maven -y
```

### Verify Maven Installation

```bash
mvn --version
```

---

## 5. Jenkins Installation

### Step 1: Add Jenkins Repository Key

```bash
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null
```

---

### Step 2: Add Jenkins Repository

```bash
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
```

---

### Step 3: Update Package Index

```bash
sudo apt update
```

---

### Step 4: Install Jenkins

```bash
sudo apt install jenkins -y
```

---

## 6. Start and Enable Jenkins Service

### Start Jenkins

```bash
sudo systemctl start jenkins
```

### Enable Jenkins at Boot

```bash
sudo systemctl enable jenkins
```

### Check Jenkins Status

```bash
sudo systemctl status jenkins
```

You should see `active (running)`.

---

## 7. Allow Jenkins Through Firewall (If UFW is Enabled)

```bash
sudo ufw allow 8080
sudo ufw reload
```

---

## 8. Access Jenkins Web Interface

Open your browser and visit:

```
http://<your-server-ip>:8080
```

---

## 9. Get Initial Admin Password

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Copy the password and paste it into the Jenkins setup screen.

---

## 10. Finish Jenkins Setup

1. Install **Suggested Plugins**
2. Create **Admin User**
3. Jenkins is now ready 🎉

---

## ✅ Jenkins Default Information

* **Service Name:** `jenkins`
* **Port:** `8080`
* **Config Path:** `/etc/default/jenkins`
* **Home Directory:** `/var/lib/jenkins`

---

==================================================================



# How to Install and Secure Tomcat 9 on Ubuntu 22.04

This guide explains **what to do**, **why you do it**, and **what each command means**, step by step.

---

## Prerequisites

* Ubuntu 22.04 LTS
* Sudo (administrator) access
* Internet connection

---

## 1. Change Hostname (Optional but Helpful)

```bash
sudo hostnamectl set-hostname Tomcat
```

### What this command does

* `hostnamectl` → manages the system’s hostname
* `set-hostname Tomcat` → renames your server to **Tomcat**
* Helps identify the server when managing multiple machines

✅ Safe and optional

---

## 2. Update System Packages

```bash
sudo apt update
```

### What this command does

* Refreshes the list of available software packages
* Ensures you install the **latest stable versions**
* Always do this before installing new software

✅ Best practice

---

## 3. Install Tomcat 9

```bash
sudo apt install tomcat9 tomcat9-docs tomcat9-admin -y
```

### What this command installs

| Package         | Purpose                     |
| --------------- | --------------------------- |
| `tomcat9`       | Core Tomcat server          |
| `tomcat9-docs`  | Official documentation      |
| `tomcat9-admin` | Manager & admin web apps    |
| `-y`            | Automatically answers “yes” |

✅ Tomcat starts automatically after install

---

## 4. Deploy Admin & Manager Applications

```bash
sudo cp -r /usr/share/tomcat9-admin/* /var/lib/tomcat9/webapps/
```

### What this command does

* Copies admin web apps into Tomcat’s active deployment directory
* Enables:

  * `/manager`
  * `/host-manager`

Without this step, admin pages may not load.

---

## 5. Create Tomcat Admin User

### Open the user configuration file

```bash
sudo nano /var/lib/tomcat9/conf/tomcat-users.xml
```

### What this file is

* Controls **who can log in**
* Defines **roles and permissions**

---

### Add roles and user

Scroll to the bottom and add **before `</tomcat-users>`**:

```xml
<role rolename="manager-script"/>
<user username="tomcat" password="admin" roles="manager-script"/>
```

### What this means (very important)

* `manager-script` → allows access to Tomcat Manager
* `username="tomcat"` → login name
* `password="admin"` → login password (change later!)

⚠️ **Never use weak passwords in production**

---

## 6. Restart Tomcat

```bash
sudo systemctl restart tomcat9
```

### What this command does

* Reloads Tomcat
* Applies configuration changes
* Required after editing config files

---

## 7. Verify Tomcat Is Running

```bash
sudo systemctl status tomcat9
```

### What to look for

```
Active: active (running)
```

❌ If not running, check logs:

```bash
sudo journalctl -u tomcat9
```

---

## 8. Allow Tomcat Through Firewall

Tomcat uses **port 8080**.

```bash
sudo ufw allow 8080
sudo ufw reload
```

### What this does

* Opens port 8080 for browser access
* Required if UFW firewall is enabled

Check firewall status:

```bash
sudo ufw status
```

---

## 9. Access Tomcat Web UI

Open a browser and go to:

```
http://<your-server-ip>:8080
```

### You should see

* **Apache Tomcat welcome page**

🎉 Tomcat is working!

---

## 10. Access Tomcat Manager & Admin Pages

### Manager App

```
http://<your-server-ip>:8080/manager
```

### Host Manager

```
http://<your-server-ip>:8080/host-manager
```

### Login credentials

* **Username:** `tomcat`
* **Password:** `admin`

---

## 11. Security Hardening (Very Important)

### 🔐 Change Default Password

Edit again:

```bash
sudo nano /var/lib/tomcat9/conf/tomcat-users.xml
```

Use a strong password:

```xml
<user username="tomcat" password="Str0ngP@ssw0rd!" roles="manager-script"/>
```

Restart:

```bash
sudo systemctl restart tomcat9
```

---

### 🔒 Restrict Manager App to Localhost (Recommended)

Edit:

```bash
sudo nano /var/lib/tomcat9/webapps/manager/META-INF/context.xml
```

Add:

```xml
<Valve className="org.apache.catalina.valves.RemoteAddrValve"
       allow="127\.0\.0\.1" />
```

### What this does

* Blocks remote access to Manager UI
* Prevents brute-force attacks
* Allows access only from the server itself

---

## 12. Common Tomcat Commands (Cheat Sheet)

| Action         | Command                          |
| -------------- | -------------------------------- |
| Start Tomcat   | `sudo systemctl start tomcat9`   |
| Stop Tomcat    | `sudo systemctl stop tomcat9`    |
| Restart Tomcat | `sudo systemctl restart tomcat9` |
| Status         | `sudo systemctl status tomcat9`  |
| View logs      | `/var/log/tomcat9/`              |

---

## 13. Important Tomcat Directories

| Purpose      | Path                                  |
| ------------ | ------------------------------------- |
| Config files | `/var/lib/tomcat9/conf/`              |
| Web apps     | `/var/lib/tomcat9/webapps/`           |
| Logs         | `/var/log/tomcat9/`                   |
| Service file | `/lib/systemd/system/tomcat9.service` |

---

## ✅ Summary

✔ Tomcat installed
✔ Web UI accessible
✔ Admin user configured
✔ Firewall configured
✔ Security hardened

---



