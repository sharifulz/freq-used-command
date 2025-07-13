# Installing Java 21 (OpenJDK) on Linux (Debian/Ubuntu-based)

Follow these steps to install Java 21 using the Temurin (Adoptium) repository on a Linux VPS.

---

## Step 1: Update System and Install Required Tools

```bash
sudo apt update
sudo apt install -y wget gnupg apt-transport-https
```

---

## Step 2: Add the Temurin GPG Key and Repository

```bash
wget -O - https://packages.adoptium.net/artifactory/api/gpg/key/public | sudo gpg --dearmor -o /usr/share/keyrings/adoptium-archive-keyring.gpg

echo "deb [signed-by=/usr/share/keyrings/adoptium-archive-keyring.gpg] https://packages.adoptium.net/artifactory/deb $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/adoptium.list
```

---

## Step 3: Install Java 21

```bash
sudo apt update
sudo apt install -y temurin-21-jdk
```

---

## Step 4: Verify Java Installation

```bash
java -version
```

Expected output:

```
openjdk version "21" 2023-09-19
OpenJDK Runtime Environment Temurin-21+35 (build 21+35)
OpenJDK 64-Bit Server VM Temurin-21+35 (build 21+35, mixed mode, sharing)
```

---

## Optional: Set Java 21 as Default (if multiple versions exist)

```bash
sudo update-alternatives --config java
sudo update-alternatives --config javac
```

---

## Notes

- This setup uses Temurin (from Adoptium), a popular and production-safe OpenJDK distribution.
- You can uninstall later using `sudo apt remove temurin-21-jdk` if needed.
