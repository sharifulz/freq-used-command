## 🗄️ Database

### PostgreSQL Tables (via Docker)
- Automatically initialized from `docker-compose` or Spring Boot `schema.sql`.

## 🧪 Testing
- JUnit for backend unit tests
- Integration tests with MockMvc
- Manual/automated testing in Flutter and Angular

## 📦 Deployment
- Docker-based containerization
- Environment variables for configuration
- Production ready builds for Angular and Flutter

## 📃 License
This project is licensed under the MIT License.

---

> **MediPilot** - Because every second counts.

# 🗺️ PostgreSQL + PostGIS Setup Guide

This guide walks you through:

1. Installing PostGIS
2. Changing a PostgreSQL user's password
3. Enabling PostGIS on a specific database
4. Creating geometry columns and viewing them in pgAdmin

---

## ✅ 1. Install PostGIS Extension

PostgreSQL + PostGIS Installation Guide for Linux Server
=========================================================

1. Update System Packages
--------------------------
Run the following command to update and upgrade all system packages:

```bash
sudo apt update && sudo apt upgrade -y
```

------------------------------------------------------------

2. Add PostgreSQL APT Repository
--------------------------------
Install required tools and add the PostgreSQL repository:

```bash
sudo apt install wget ca-certificates -y
wget -qO - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" | \
sudo tee /etc/apt/sources.list.d/pgdg.list
```

Then update package lists again:

```bash
sudo apt update
```

------------------------------------------------------------

3. Install PostgreSQL and PostGIS
----------------------------------
Install PostgreSQL 16 with PostGIS:

```bash
sudo apt install postgresql-16 postgresql-16-postgis-3 postgresql-16-postgis-3-scripts -y
```

Check version:

```bash
    psql --version
```
------------------------------------------------------------

4. Start and Enable PostgreSQL
------------------------------
Enable PostgreSQL to start on boot and start it now:

```bash
sudo systemctl start postgresql
sudo systemctl enable postgresql
```

------------------------------------------------------------

5. Create Database and Enable PostGIS
-------------------------------------
Switch to PostgreSQL user and enter psql:

```bash
    sudo -i -u postgres
    psql
```

Inside psql, run:

```bash
CREATE DATABASE medipilot;
\c gisdb
CREATE EXTENSION postgis;
CREATE EXTENSION postgis_topology;
\dx
```
Exit:

```bash
exit
```

------------------------------------------------------------

6. Configure PostgreSQL for External Connections (Optional)
------------------------------------------------------------
Edit postgresql.conf:

```bash
sudo nano /etc/postgresql/16/main/postgresql.conf
```

Set:
listen_addresses = '*'

Edit pg_hba.conf:

```bash
sudo nano /etc/postgresql/16/main/pg_hba.conf
```

Add this line:

```bash
host    all             all             0.0.0.0/0               md5
```

Restart service:

```bash
sudo systemctl restart postgresql
```
------------------------------------------------------------

7. Install and Configure UFW Firewall
-------------------------------------
Install UFW:

```bash
sudo apt install ufw -y
```

Allow PostgreSQL and SSH:

```bash
sudo ufw allow 5432/tcp
sudo ufw allow OpenSSH
sudo ufw enable
```

Check status:

```bash
sudo ufw status
```

------------------------------------------------------------

8. Verify PostGIS is Working
-----------------------------
Log in and run:

```bash
sudo -i -u postgres
psql -d gisdb
SELECT PostGIS_Full_Version();
```

Expected output:
    POSTGIS="3.x.x" [plus GEOS, PROJ, GDAL versions...]

9. 
------------------------------------------------------------
Method 1: Change Password Inside psql Shell
🔹 Step-by-Step:

Switch to the postgres Linux user:

```bash
sudo -i -u postgres
```

Enter the PostgreSQL interactive shell:

```bash
psql
```

Run the SQL to update the password:

```bash
ALTER USER your_username WITH PASSWORD 'your_new_password';
```

Exit the shell:

```bash
exit
```

