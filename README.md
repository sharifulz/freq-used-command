
# 🛡️ MySQL Backup Script with SMS Notification

This guide documents how to use and schedule a MySQL backup script with SMS notification using a cron job on a Linux server.

---

## 📂 Script Location

Your backup script is located at:
```
/opt/script/mysqlbackupscript.sh
```

Ensure it's executable:
```bash
chmod +x /opt/script/mysqlbackupscript.sh
```

---

## 🧪 Step 1: Run the Script Manually (Test)

Run manually to test:
```bash
bash /opt/script/mysqlbackupscript.sh
```

Check for:
- The backup file inside `/opt/db_backup`
- SMS message (Success or Failure)
- Logs in `/tmp/backup_error.log` (only if there’s an error)

---

## 🕰️ Step 2: Schedule with Cron

Edit crontab:
```bash
crontab -e
```

Add the following line to run every day at 10:00 PM:

```
0 22 * * * /opt/script/mysqlbackupscript.sh >> /var/log/mysqlbackup.log 2>&1
```

Here’s what each field means:

| Field         | Value  | Meaning                              |
|---------------|--------|--------------------------------------|
| Minute        | `0`    | At 0 minutes                         |
| Hour          | `22`   | At 10 PM (24-hour format)            |
| Day of Month  | `*`    | Every day of the month               |
| Month         | `*`    | Every month                          |
| Day of Week   | `*`    | Every day of the week                |
---

## 📝 Step 3: View Current Cron Jobs

To list all cron jobs for current user:
```bash
crontab -l
```

---

## 🐾 Step 4: View Cron File Contents

To view cron jobs (raw file):
```bash
cat /var/spool/cron/crontabs/$(whoami)
```

> Note: May require `sudo` on some systems.

---

## 🔍 Step 5: Check Cron Service Status

For Ubuntu/Debian:
```bash
systemctl status cron
```

To start/enable if not running:
```bash
sudo systemctl start cron
sudo systemctl enable cron
```

---

## 🚫 Step 6: Disable or Remove Cron Job

### Temporarily comment the line:
Edit via:
```bash
crontab -e
```

Add `#` in front of the line to disable.

### Or completely remove all cron jobs:
```bash
crontab -r
```

⚠️ **This removes all cron jobs for the current user.**

---

## 💡 Tips

- Always use absolute paths in scripts.
- Ensure MySQL credentials are correct.
- Monitor `/var/log/mysqlbackup.log` for job logs.
- Confirm SMS API credits/limits for reliable notification.

---

✨ Your MySQL backups now walk with discipline, reporting success or failure as faithfully as a town crier.
