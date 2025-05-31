# 03 – SSH Hardening

> Secure SSH access by disabling root login, enforcing key-based auth, and changing default port.

---

## Objectives

- Disable root login
- Change default SSH port (optional)
- Enable only key-based login
- Add a non-root admin user with sudo
- Test all changes safely

---

## Commands Used

```bash
sudo adduser yourusername
sudo usermod -aG sudo yourusername
sudo cp -r ~/.ssh /home/yourusername/
sudo chown -R yourusername:yourusername /home/yourusername/.ssh

sudo nano /etc/ssh/sshd_config
sudo systemctl restart ssh
```

---

## Steps

### 1. Create a New Admin User (No Root Login)

On your **Ubuntu Server VM**:
```bash
sudo adduser sysadmin
```
Follow the prompts to set a password.

Then give it sudo access:
```bash
sudo usermod -aG sudo sysadmin
```

---

### 2. Copy your SSH Key to sysadmin User

From your current user:
```bash
sudo cp -r ~/.ssh /home/sysadmin/
sudo chown -R sysadmin:sysadmin /home/sysadmin/.ssh
```
Optinal: Test key permissions:
```bash
sudo chmod 700 /home/sysadmin/.ssh
sudo chmod 600 /home/sysadmin/.ssh/authorized_keys
```

---

### 3. Harden sshd_config

Edit the SSH daemon config:
```bash
sudo nano /etc/ssh/sshd_config
```

Find and Uncommented options to override the to default value:
```ini
# Don't allow root login
PermitRootLogin no

# Optional – change default port (e.g. 2222)
Port 2222

# Only allow key-based login
PasswordAuthentication no

# Optional – allow only the new sysadmin
AllowUsers sysadmin
```

---

### 4. restart SSH Service

```bash
sudo systemctl restart ssh
```

---

### 5. Test Login from Host (before loggid out of current session)

On your host machine:
```bash
ssh sysadmin@your_server_ip -p 2222
```
You should log in via key without password prompt
Root login and password login should now be blocked