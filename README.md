# 📦 Database Backup & Restore Commands (PostgreSQL, MySQL, MSSQL)

This guide contains essential commands for backing up and restoring databases for PostgreSQL, MySQL, and Microsoft SQL Server (MSSQL), both locally and to/from remote servers.

---

## 🐘 PostgreSQL

### Backup (Local Server)

```bash
pg_dump -U postgres -d your_database -F c -f /path/to/backup/your_database.backup
```

### Backup (Remote Server)

```bash
pg_dump -h remote_ip -p 5432 -U postgres -d your_database -F c -f your_database.backup
```

### Restore (Same or Different Server)

```bash
pg_restore -U postgres -d your_database -F c /path/to/your_database.backup
```

To create the target database before restoring:

```bash
createdb -U postgres your_database
```

---

## 🐬 MySQL / MariaDB

### Backup (Local Server)

```bash
mysqldump -u root -p your_database > /path/to/backup/your_database.sql
```

### Backup (Remote Server)

```bash
mysqldump -h remote_ip -P 3306 -u root -p your_database > your_database.sql
```

### Restore (Same or Different Server)

```bash
mysql -u root -p your_database < /path/to/your_database.sql
```

If the database doesn't exist yet:

```bash
mysql -u root -p -e "CREATE DATABASE your_database;"
```

---

## 🪟 MSSQL (SQL Server on Linux or Windows)

### Backup (Linux using `sqlcmd`)

```bash
sqlcmd -S localhost -U sa -P 'your_password' -Q "BACKUP DATABASE [your_database] TO DISK = N'/var/opt/mssql/backup/your_database.bak' WITH INIT;"
```

### Backup (Remote Server)

```bash
sqlcmd -S remote_ip,1433 -U sa -P 'your_password' -Q "BACKUP DATABASE [your_database] TO DISK = N'/var/opt/mssql/backup/your_database.bak' WITH INIT;"
```

### Restore (Local or Remote)

```bash
sqlcmd -S localhost -U sa -P 'your_password' -Q "RESTORE DATABASE [your_database] FROM DISK = N'/var/opt/mssql/backup/your_database.bak' WITH REPLACE;"
```

---

## 📝 Notes

- Replace `your_database`, `root`, `postgres`, and `sa` with your actual values.
- Make sure backup paths exist and have proper write permissions.
- Use `.sql` for logical backups (MySQL), `.bak` for SQL Server, `.backup` or `.dump` for PostgreSQL.

---

🛡️ Always test your backups regularly to ensure successful recovery during emergencies.
