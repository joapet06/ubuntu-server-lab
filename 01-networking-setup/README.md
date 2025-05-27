# 01 - Static IP Address Configuration

## Objective

First, we need to configure a static IP address on Ubuntu Server 24.04.2 LTS using netplan to ensure consistent remote access and network stability within the virtual lab environment.

---

## Environment

| Component        | Description                     |
|------------------|---------------------------------|
| OS               | Ubuntu Server 24.04.2 LTS       |
| Hypervisor       | VirtualBox                      |
| Network Adapter  | Bridged / Host-Only (recommended) |
| Interface Name   | `enp0s3` (may vary)             |

---

## Steps

### 1. Identify Network Interface

```bash
ip a
```
```bash
sysadmin@ubuntuvm:~$ ip a
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

Locate your primary interface (e.g., enp0s3, ens18, etc.).

---

### 2. Backup Current Netplan Configuration

```bash
sudo cp /etc/netplan/original-netplan.yaml /etc/netplan/original-netplan.yaml.backup
```

Your Netplan .yaml file may have a different name—adjust the filename accordingly to match your system.

---

### 3. Edit Netplan Config File

Use Nano or other text editor of your preference to edit the .yaml file.
```bash
sudo nano /etc/netplan/50-cloud-init.yaml
```

In my case the file name is 50-cloud-init.yaml

Example static IP configuration:
```yaml
network:
  version: 2
  ethernets:
    enp0s3:
      dhcp4: no
      addresses:
        - 192.168.100.220/24
      routes:
        - to: default
          via: 192.168.100.1
      nameservers:
       addresses: [8.8.8.8, 1.1.1.1]
```

Adjust:
'enp0s3' to match your actual interface.

Addresses to your desired IP and subnet.

'via:' to your router/gateway IP.

---

### 4. Apply the Configuration

```bash
sudo netplan apply
```

Verify:
```bash
ip a
ping -c 3 8.8.8.8
```

---

## Testing

|Command     |Purpose                   |
|------------|--------------------------|
|ip a        |Check IP address          |
|Ping 8.8.8.8|Test internet connectivity|
|hostname -I |Confirm static IP         |