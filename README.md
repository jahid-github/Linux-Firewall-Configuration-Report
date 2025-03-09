# Linux-Firewall-Configuration-Report

## Introduction
This document outlines the firewall configuration for securing a Linux server. The firewall is set to:
- Allow SSH access for remote management.
- Allow HTTP and HTTPS traffic for web services.
- Block all other incoming traffic.
- Log all allowed and blocked traffic.
- Implement security measures against SYN flood and brute-force SSH attacks.

## Firewall Configuration

### 1. Allow SSH (OpenSSH)
```bash
sudo ufw allow OpenSSH
```
**Reason:** This rule allows SSH connections so that administrators can remotely manage the server.

### 2. Allow HTTP and HTTPS Traffic
```bash
sudo ufw allow 80/tcp    # HTTP
sudo ufw allow 443/tcp   # HTTPS
```
**Reason:** These rules allow incoming web traffic to ensure users can access the website hosted on the server.

### 3. Block All Other Incoming Traffic
```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
```
**Reason:** By default, the firewall blocks all incoming traffic except the explicitly allowed ones, preventing unauthorized access.

### 4. Enable Logging
```bash
sudo ufw logging medium
```
**Reason:** This setting logs all denied packets and allowed connections for monitoring and security auditing.

### 5. Protect Against SYN Flood Attacks
```bash
sudo iptables -A INPUT -p tcp --syn -m limit --limit 1/s --limit-burst 3 -j ACCEPT
```
**Reason:** This rule limits the rate of SYN packets per second, helping to mitigate SYN flood attacks that attempt to overwhelm the server with incomplete TCP connection requests.

### 6. Protect Against Brute-Force SSH Attacks Using Fail2Ban
#### Install and Enable Fail2Ban:
```bash
sudo apt install fail2ban -y
sudo systemctl enable fail2ban
sudo systemctl start fail2ban
```

#### Configure SSH Protection in `/etc/fail2ban/jail.local`:
```bash
[sshd]
enabled = true
port = ssh
maxretry = 5
findtime = 10m
bantime = 1h
```
**Reason:** Fail2Ban monitors failed SSH login attempts and temporarily bans IPs that exceed the allowed number of retries, preventing brute-force attacks.

### 7. Enable the Firewall
```bash
sudo ufw enable
```
**Reason:** This activates the firewall and enforces all defined rules.

## Conclusion
The configured firewall enhances security by allowing only essential services, logging traffic, and mitigating common network attacks such as SYN floods and brute-force login attempts. Continuous monitoring and periodic rule updates are recommended for optimal security.

---


