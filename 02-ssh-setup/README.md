# 02 – SSH Setup on Ubuntu Server

In this step, we’ll install and configure OpenSSH Server on Ubuntu Server 24.04.2 LTS to enable secure remote access.

---

## Objectives

- Install `openssh-server`
- Verify the SSH service is running
- Connect via SSH from the host
- Optional: change default port (covered in step 03)

---

## Commands Used

```bash
sudo apt update
sudo apt install openssh-server -y
sudo systemctl status ssh
ip a  # get server IP
```
---

## Steps

### 1.Install OpenSSH Server

```bash
sudo apt update
sudo apt install openssh-server -y
```

---

### 2.Check SSH Service Status

```bash
sudo systemctl status ssh
```
My output:
```bash
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/usr/lib/systemd/system/ssh.service; enabled; preset: enabled)
     Active: active (running) since Sun 2025-05-25 14:36:06 UTC; 6h ago
TriggeredBy: ● ssh.socket
       Docs: man:sshd(8)
             man:sshd_config(5)
    Process: 750 ExecStartPre=/usr/sbin/sshd -t (code=exited, status=0/SUCCESS)
   Main PID: 755 (sshd)
      Tasks: 1 (limit: 2272)
     Memory: 4.1M (peak: 20.5M)
        CPU: 76ms
     CGroup: /system.slice/ssh.service
             └─755 "sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups"

May 25 14:36:06 ubuntuvm systemd[1]: Starting ssh.service - OpenBSD Secure Shell server...
May 25 14:36:06 ubuntuvm sshd[755]: Server listening on :: port 2222.
May 25 14:36:06 ubuntuvm systemd[1]: Started ssh.service - OpenBSD Secure Shell server.
May 25 19:45:30 ubuntuvm sshd[1632]: Accepted password for sysadmin from 192.168.100.99 port 54438 ssh2
May 25 19:45:30 ubuntuvm sshd[1632]: pam_unix(sshd:session): session opened for user sysadmin(uid=1001) by sysadmin(uid=0)
```
If it's active and running, we're good.

---

### 3.Find Server IP Address

```bash
ip a
```
```bash
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:3f:a1:6b brd ff:ff:ff:ff:ff:ff
    inet 192.168.100.220/24 brd 192.168.100.255 scope global enp0s3
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe3f:a16b/64 scope link 
       valid_lft forever preferred_lft forever
```
Note the IP (in my case it's 192.168.100.220). Change yours accordingly.

---

### 4.Test Access from Host Machine

On your host:

```bash
ssh username@192.168.100.220
```
Replacce username with your server user and IP with your actual server IP.

---

## Verify

- ssh command connects to server
- Server prompts for password

---

## Notes

- SSH is essential for remote server management
- In our next step we will cover SSH hardening (disabling password login, root login, changing port, etc.)