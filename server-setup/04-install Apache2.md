# Complete Apache2 Installation Guide for Ubuntu 24.04 VPS (Fixed)

## 1. Update System
```bash
sudo apt update && sudo apt upgrade -y
```

## 2. Install Apache2
```bash
sudo apt install apache2 -y
```

## 3. Start and Enable Apache2
```bash
sudo systemctl start apache2
sudo systemctl enable apache2
```

## 4. Verify Apache2 is Running
```bash
sudo systemctl status apache2
```
Expected: `active (running)`

Test in browser: `http://your-server-ip`

## 5. Configure Firewall (UFW)
```bash
sudo ufw allow 'Apache Full'
sudo ufw reload
sudo ufw status
```

## 6. Secure Configuration (Directory Listing Fix)

```bash
sudo nano /etc/apache2/apache2.conf
```

**Find line ~171** (Directory /var/www/ block) and **replace with:**
```
<Directory /var/www/>
    Options -Indexes +FollowSymLinks
    AllowOverride None
    Require all granted
</Directory>
```

**Test config BEFORE restarting:**
```bash
sudo apache2ctl configtest
```
Expected: `Syntax OK`

**Restart Apache:**
```bash
sudo systemctl restart apache2
```

## 7. Verify Security Fix
```bash
curl http://localhost
```
Should show Apache default page, **no directory listing**

## 8. Useful Commands
```bash
# Status
sudo systemctl status apache2

# Restart
sudo systemctl restart apache2

# Reload config
sudo systemctl reload apache2

# Test config
sudo apache2ctl configtest

# Logs
sudo tail -f /var/log/apache2/error.log
```

## 9. Default Locations
```
Document Root: /var/www/html/
Config: /etc/apache2/apache2.conf
Sites: /etc/apache2/sites-available/
Logs: /var/log/apache2/
```

## Troubleshooting
```
# Config error? Check:
sudo apache2ctl configtest

# Service down?
sudo systemctl status apache2

# Firewall blocking?
sudo ufw status
```

**✅ Apache2 is now installed, secured, and running perfectly on Ubuntu 24.04!** Access via `http://your-server-ip`.[1]

[1](https://linuxconfig.org/how-to-install-node-js-on-ubuntu-24-04)
