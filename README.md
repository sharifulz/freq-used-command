# Basic Linux Firewall (UFW) Commands

This README provides essential commands for managing the UFW (Uncomplicated Firewall) on Linux servers.

---

## 🔍 Check UFW Status

```bash
sudo ufw status
```

Check whether the firewall is active and which ports are open.

---

## 🔥 Enable UFW Firewall

```bash
sudo ufw enable
```

This command activates the firewall. It will start blocking unauthorized traffic based on current rules.

---

## 🧯 Disable UFW Firewall

```bash
sudo ufw disable
```

This disables all firewall protection (use with caution).

---

## 🚪 Allow Specific Port (e.g., 8080)

```bash
sudo ufw allow 8080/tcp
```

Open port 8080 for TCP connections.

---

## 🔒 Deny Specific Port (e.g., 8080)

```bash
sudo ufw deny 8080/tcp
```

Close port 8080 for TCP connections.

---

## 🌐 Allow Access from Specific IP to a Port

```bash
sudo ufw allow from 192.168.1.100 to any port 22
```

Allows only IP `192.168.1.100` to access SSH (port 22).

---

## 🧾 View UFW Rules (with Numbers)

```bash
sudo ufw status numbered
```

Shows firewall rules with line numbers for easy deletion.

---

## 🧹 Delete a Specific Rule

```bash
sudo ufw delete [rule_number]
```

Example:

```bash
sudo ufw delete 2
```

Deletes rule number 2 from the rule list.

---

## 🔄 Reload UFW (Apply Changes)

```bash
sudo ufw reload
```

Reloads the firewall with updated rules.

---

## 🧠 Notes

- Always allow SSH (`sudo ufw allow OpenSSH`) before enabling the firewall to avoid locking yourself out.
- UFW is available by default on Ubuntu/Debian but may require installation:

```bash
sudo apt install ufw
```
