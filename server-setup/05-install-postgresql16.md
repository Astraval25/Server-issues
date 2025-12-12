# Complete PostgreSQL 16 Installation & Setup Guide for Ubuntu 24.04 VPS

## 1. Update System
```bash
sudo apt update && sudo apt upgrade -y
```

## 2. Install PostgreSQL 16
```bash
sudo apt install postgresql-16 postgresql-contrib-16 -y
```

## 3. Start and Enable Service
```bash
sudo systemctl start postgresql
sudo systemctl enable postgresql
sudo systemctl status postgresql
```
Expected: `active (running)`

## 4. Set PostgreSQL Password
```bash
sudo -u postgres psql
```
**Inside psql:**
```sql
ALTER USER postgres PASSWORD 'your_strong_password_here';
\q
```

## 5. Configure Remote Access (postgresql.conf)
```bash
sudo nano /etc/postgresql/16/main/postgresql.conf
```

**Find and uncomment/change these lines:**
```
listen_addresses = '*'          # Line ~88
#port = 5432                   # Already default
```

## 6. Configure Authentication (pg_hba.conf) - **CRITICAL**
```bash
sudo nano /etc/postgresql/16/main/pg_hba.conf
```

**REPLACE ENTIRE CONTENT with this EXACT config:**

```
# Database administrative login by Unix domain socket
local   all             postgres                                peer

# "local" is for Unix domain socket connections only
local   all             all                                     md5

# IPv4 local connections:
host    all             all             127.0.0.1/32            md5

# IPv6 local connections:
host    all             all             ::1/128                 md5

# REMOTE ACCESS - Allow ANY IP with password (CRITICAL - NEAR TOP)
host    all             all             0.0.0.0/0               md5
host    all             all             ::/0                    md5

# Replication connections
local   replication     all                                     peer
host    replication     all             127.0.0.1/32            md5
host    replication     all             ::1/128                 md5
host    replication     all             0.0.0.0/0               md5
```

## 7. Restart PostgreSQL
```bash
sudo systemctl restart postgresql
sudo systemctl status postgresql
```

## 8. Configure Firewall
```bash
sudo ufw allow 5432/tcp
sudo ufw reload
sudo ufw status
```

## 9. Test All Connections

### Local Test
```bash
sudo -u postgres psql
```
**Inside psql:** `\q`

### Password Test (Local)
```bash
psql -U postgres -h localhost
```
Enter password when prompted.

### Remote Test (from another machine)
```bash
psql -h your-server-ip -U postgres -d postgres
```

## 10. Create Database & User (Example)
```bash
sudo -u postgres psql
```
```sql
CREATE DATABASE myapp;
CREATE USER myuser WITH PASSWORD 'mypassword';
GRANT ALL PRIVILEGES ON DATABASE myapp TO myuser;
\q
```

## 11. Verify Everything
```bash
# Service status
sudo systemctl status postgresql

# Listening on port
sudo netstat -tlnp | grep 5432

# Version
psql --version

# Processes
sudo systemctl status postgresql@*-main
```

## Default Locations
```
Data:        /var/lib/postgresql/16/main/
Config:      /etc/postgresql/16/main/
Logs:        /var/log/postgresql/
Socket:      /var/run/postgresql/
```

## Useful Commands
```bash
# Connect
sudo -u postgres psql

# Restart
sudo systemctl restart postgresql

# Logs
sudo tail -f /var/log/postgresql/postgresql-16-main.log

# Backup
sudo -u postgres pg_dump myapp > backup.sql
```

## ⚠️ Security Warning
- **`0.0.0.0/0` allows worldwide access** - Use **STRONG** passwords
- Production: Use SSL, VPN, or specific IPs (`192.168.1.0/24`)
- Change default `postgres` password immediately

**✅ PostgreSQL 16 is fully installed with remote access enabled!**[1]

[1](https://linuxconfig.org/how-to-install-node-js-on-ubuntu-24-04)
