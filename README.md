
# 🧙‍♂️ MSSQL Backup Script Setup on Ubuntu/Debian Linux

This guide explains how to install `sqlcmd` tools on a Linux server (Ubuntu/Debian), and how to run a full MSSQL database backup using the command line.

---

## 🔧 Step 1: Install `sqlcmd` Tools

### 1. Add Microsoft Package Repository

```
curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
sudo add-apt-repository "$(curl https://packages.microsoft.com/config/ubuntu/$(lsb_release -rs)/prod.list)"
```

### 2. Update Package Index

```
sudo apt-get update
```

### 3. Install SQL Server Tools

```
sudo apt-get install -y mssql-tools unixodbc-dev
```

### 4. Add to PATH Permanently

```
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
```

---

## ✅ Step 2: Confirm Installation

To confirm the tools are installed correctly, run:

```
which sqlcmd
```

You should see something like:

```
/opt/mssql-tools/bin/sqlcmd
```

---

## 💽 Step 3: Run MSSQL Full Backup

Once `sqlcmd` is installed, run the backup command:

```
/opt/mssql-tools/bin/sqlcmd -S localhost -U username -P 'your-password' -Q "BACKUP DATABASE [TaskTracker] TO DISK = N'/var/opt/mssql/backups/TaskTracker_FULL_$(date +%Y%m%d).bak' WITH FORMAT, INIT, SKIP, NOREWIND, NOUNLOAD, STATS = 10"
```

> 💡 **Important:** If using this command inside a shell script, the `$(date +%Y%m%d)` must be resolved by the shell. You need to inject the date beforehand using a variable.

---

## 📜 Note

You may create a shell script and schedule it via `cron` for regular backups. If you'd like a full script with rotation and logs, let us know!

---

🛡️ Happy Backing Up!
