# 04 – UFW Firewall Configuration

> In this step, we configure UFW (Uncomplicated Firewall) to protect the server by allowing only necessary ports and services.

---

## Objectives

- Enable UFW
- Allow SSH (non-default port if changed)
- Allow essential services (e.g. HTTP/HTTPS)
- Deny everything else by default
- Make UFW start on boot

---

## Commands Used

```bash
sudo apt install ufw -y
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 2222/tcp  # Adjust if your SSH port is custom
sudo ufw allow http
sudo ufw allow https
sudo ufw enable
sudo ufw status verbose
```

---

## Steps

### 1. Install and Check UFW

```bash
sudo apt update
sudo apt install ufw -y
sudo ufw status verbose
```
Expected output: Status: inactive (at first).

---

### 2. Configure UFW Rules

#### 2.1 Default Policy
```bash
sudo ufw default deny incoming
sudo ufw default allow outging
```

#### 2.2 Allow SSH Port
If you are using default SSH, use port 22:
```bash
sudo ufw allow ssh
```
If you changed SSH to a custom port (mine is 2222), do:
```bash
sudo ufw allow 2222/tcp
```

#### 2.3 Optional: Allow Web Traffic
```bash
sudo ufw allow http
sudo ufw allow https
```
You can skip this if your server isn't hosting websites.

---

### 3. Enable UFW

```bash
sudo ufw enable
```
Confirm wih y.

Check status:
```bash
sudo ufw status verbose
```
```bash
sysadmin@ubuntuvm:~$ sudo ufw status verbose
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)
New profiles: skip

To                         Action      From
--                         ------      ----
2222/tcp                   ALLOW IN    Anywhere                  
2222/tcp (v6)              ALLOW IN    Anywhere (v6)
```

---

### 4. Add Rule Descriptions (Optional)

```bash
sudo ufw allow 2222/tcp comment 'Allow SSH on custom port'
sudo ufw allow http comment 'Allow HHTP for web services'
sudo ufw allow https comment 'Allow HTTPS for secure web traffic'
```