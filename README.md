

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

