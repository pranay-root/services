# services

Here’s a **detailed guide** on installing, accessing, and setting up the Common services for creating CTF.  

---

## **1. SSH (Secure Shell)**
### **🔹 Install SSH**
```bash
sudo apt update
sudo apt install openssh-server -y
```

### **🔹 Start & Enable SSH**
```bash
sudo systemctl enable --now ssh
```

### **🔹 Access SSH**
- **From Linux/macOS:**
  ```bash
  ssh username@server-ip
  ```
- **From Windows:** Use **PuTTY** or PowerShell:
  ```powershell
  ssh username@server-ip
  ```

### **🔹 Setup SSH**
- **Change SSH Port (optional):**
  ```bash
  sudo nano /etc/ssh/sshd_config
  ```
  - Change `#Port 22` → `Port 2222` (for example).
  - Restart SSH: `sudo systemctl restart ssh`
  
---

## **2. Web Server (Apache)**
### **🔹 Install Apache**
```bash
sudo apt install apache2 -y
```

### **🔹 Start & Enable Apache**
```bash
sudo systemctl enable --now apache2
```

### **🔹 Access Apache**
- Open a browser and go to:
  ```
  http://server-ip
  ```

### **🔹 Setup Apache**
- **Configure Virtual Hosts:**
  ```bash
  sudo nano /etc/apache2/sites-available/000-default.conf
  ```
  Add:
  ```xml
  <VirtualHost *:80>
      ServerAdmin admin@example.com
      DocumentRoot /var/www/html
      ErrorLog ${APACHE_LOG_DIR}/error.log
      CustomLog ${APACHE_LOG_DIR}/access.log combined
  </VirtualHost>
  ```
  Save and restart Apache:
  ```bash
  sudo systemctl restart apache2
  ```

---

## **3. SMB (Samba - Port 445)**
### **🔹 Install Samba**
```bash
sudo apt install samba -y
```

### **🔹 Start & Enable Samba**
```bash
sudo systemctl enable --now smbd
```

### **🔹 Access SMB**
- **From Windows:** Open `\\server-ip\`
- **From Linux:** Mount the share:
  ```bash
  smbclient -L //server-ip -U username
  ```

### **🔹 Setup Samba**
- **Create a Share:**
  ```bash
  sudo nano /etc/samba/smb.conf
  ```
  Add:
  ```
  [public]
      path = /srv/samba/public
      browsable = yes
      writable = yes
      guest ok = yes
  ```
  Restart Samba:
  ```bash
  sudo systemctl restart smbd
  ```

---

## **4. MySQL (Database Server)**
### **🔹 Install MySQL**
```bash
sudo apt install mysql-server -y
```

### **🔹 Start & Enable MySQL**
```bash
sudo systemctl enable --now mysql
```

### **🔹 Access MySQL**
- Log in as root:
  ```bash
  mysql -u root -p
  ```

### **🔹 Setup MySQL**
- Secure MySQL installation:
  ```bash
  sudo mysql_secure_installation
  ```
- **Create a Database & User:**
  ```sql
  CREATE DATABASE testdb;
  CREATE USER 'testuser'@'%' IDENTIFIED BY 'password';
  GRANT ALL PRIVILEGES ON testdb.* TO 'testuser'@'%';
  FLUSH PRIVILEGES;
  ```
- **Allow Remote Connections:**
  ```bash
  sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
  ```
  Change:
  ```
  bind-address = 0.0.0.0
  ```
  Restart MySQL:
  ```bash
  sudo systemctl restart mysql
  ```

---

## **5. RDP (Remote Desktop Protocol)**
### **🔹 Install XRDP**
```bash
sudo apt install xrdp -y
```

### **🔹 Start & Enable XRDP**
```bash
sudo systemctl enable --now xrdp
```

### **🔹 Access RDP**
- **From Windows:** Open `Remote Desktop Connection`, enter `server-ip`.
- **From Linux:** Use:
  ```bash
  rdesktop server-ip
  ```

### **🔹 Setup XRDP**
- **Allow RDP Through Firewall:**
  ```bash
  sudo ufw allow 3389/tcp
  ```
- Restart XRDP:
  ```bash
  sudo systemctl restart xrdp
  ```

---

## **6. Telnet**
### **🔹 Install Telnet**
```bash
sudo apt install telnetd -y
```

### **🔹 Start & Enable Telnet**
```bash
sudo systemctl enable --now inetd
```

### **🔹 Access Telnet**
- **From Linux:**  
  ```bash
  telnet server-ip
  ```
- **From Windows:** Open `Command Prompt`:
  ```powershell
  telnet server-ip
  ```

### **🔹 Setup Telnet**
- **Allow Remote Connections:**
  ```bash
  sudo nano /etc/inetd.conf
  ```
  Uncomment the Telnet line and restart:
  ```bash
  sudo systemctl restart inetd
  ```

---

## **7. FTP (File Transfer Protocol)**
### **🔹 Install vsftpd**
```bash
sudo apt install vsftpd -y
```

### **🔹 Start & Enable FTP**
```bash
sudo systemctl enable --now vsftpd
```

### **🔹 Access FTP**
- **From Linux:**  
  ```bash
  ftp server-ip
  ```
- **From Windows:** Open `File Explorer` and enter:
  ```
  ftp://server-ip
  ```

### **🔹 Setup FTP**
- **Enable Anonymous Access (Optional):**
  ```bash
  sudo nano /etc/vsftpd.conf
  ```
  Change:
  ```
  anonymous_enable=YES
  ```
  Restart FTP:
  ```bash
  sudo systemctl restart vsftpd
  ```

---

### **🚀 Final Notes**
- Ensure **firewall rules** allow required ports:
  ```bash
  sudo ufw allow 22,80,443,445,3306,3389,23,21/tcp
  sudo ufw enable
  ```

