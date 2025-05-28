
# Git Master Guide 🚀

This documentation covers essential Git workflows, configurations (PAT & SSH), and advanced usage tips including hooks and multi-account SSH setups.

---

## 📁 Git Concepts & Workflow

### Git Directory Structure
- **Working Directory**: Your local files.
- **Staging Area (Index)**: Files prepared for the next commit.
- **Commit History**: Saved snapshots of your work.
- **Stash**: Temporary store of changes.
- **Remote**: Central repo (like GitHub/GitLab).

---

## 🚀 Basic Git Workflow

### Step 1: Initialize & Setup Branches

```bash
git init
git branch br1
git branch br2
git checkout br1
```

### Step 2: Work on `br1`

```bash
echo "Hello from br1" > file.txt
git add file.txt
git commit -m "br1: first commit"
# Repeat commit steps 3x
```

### Step 3: Work on `br2`

```bash
git checkout br2
echo "Hello from br2" > file.txt
git add file.txt
git commit -m "br2: first commit"
# Repeat commit steps 3x
```

### Step 4: Merge Branch to Main

```bash
git checkout main
git merge br1
```

### Step 5: Cherry-pick Specific Commit

```bash
git cherry-pick <commit-hash>
```

---

## 🔁 Advanced Commands

### Rebase

```bash
git rebase main
# Makes commit history linear
```

### Revert (safe for public branches)

```bash
git revert <commit-hash>
```

### Reset (dangerous, modifies history)

```bash
git reset --soft HEAD~1   # Keep staged
git reset --mixed HEAD~1  # Keep unstaged
git reset --hard HEAD~1   # Discard all
```

---

## 🌐 Remote Repos

### Set up Remote

```bash
git remote add origin git@github.com-account1:username/repo.git
git push -u origin main
```

### View Logs

```bash
git log --all --oneline
```

---

## 🔑 SSH-Based Git Setup (Multi-Account)

### Step 1: Generate SSH Keys

```bash
ssh-keygen -t rsa -b 4096 -C "email@example.com" -f ~/.ssh/id_rsa_account1
```

Repeat for each GitHub/GitLab account.

### Step 2: Start SSH Agent

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa_account1
```

### Step 3: Configure SSH

Edit `~/.ssh/config`:

```
# GitHub Account 1
Host github.com-account1
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa_account1
  IdentitiesOnly yes

# GitLab Account 2
Host gitlab.com-account2
  HostName gitlab.com
  User git
  IdentityFile ~/.ssh/id_rsa_account2
  IdentitiesOnly yes
```

> 💡 Don’t forget:
```bash
chmod 600 ~/.ssh/config
```

### Step 4: Clone Using Custom Host

```bash
git clone git@github.com-account1:username/repo.git
git clone git@gitlab.com-account2:username/repo.git
```

---

## 📤 Push Local Repo to Remote

```bash
git init
git add .
git commit -m "Initial commit"
git remote add origin git@github.com-account1:username/repo.git
git push -u origin main
```

---

## ✅ Git Hooks

### 🔍 Pre-commit Hook: Block `System.out.println`

1. Create the hook:
```bash
touch .git/hooks/pre-commit
chmod +x .git/hooks/pre-commit
```

2. Script content:

```bash
#!/bin/bash

echo "🔍 Running pre-commit checks..."

files=$(git diff --cached --name-only --diff-filter=ACM | grep '\.java$')

for file in $files; do
  if grep -q "System.out.println" "$file"; then
    echo "🚫 Error: 'System.out.println' found in $file"
    exit 1
  fi
done

echo "✅ Pre-commit checks passed."
exit 0
```

### 🔒 Additional Pre-commit Validations

#### a. Block TODO/FIXME

```bash
grep -rnw 'src/' -e 'TODO\|FIXME' && echo "Remove TODOs before commit!" && exit 1
```

#### b. Prevent Large Files (>5MB)

```bash
find . -size +5M | grep -q . && echo "Too large files found!" && exit 1
```

---

### 📜 Commit-msg Hook: Enforce Format

1. Create `.git/hooks/commit-msg`

```bash
touch .git/hooks/commit-msg
chmod +x .git/hooks/commit-msg
```

2. Add script:

```bash
#!/bin/bash

pattern="^(feat|fix|docs|style|refactor|test|chore)\([a-z]+\): .{10,}$"
if ! grep -qE "$pattern" "$1"; then
  echo "❌ Commit message format is invalid!"
  exit 1
fi
```

---

## 🛠 Miscellaneous

- Set default branch to `main`:
```bash
git config --global init.defaultBranch main
```

- Checkout existing branch:
```bash
git checkout br1
```

---

## 📂 Sample Repositories Cloned Using SSH

```bash
git clone git@github.com-account1:shariful-w3/microservices-config.git
git clone git@gitlab.com-account2:shaarifulz/portableportal.git
```
---
### 🔗 Set Remote Repository Using SSH Config

Step 3: Add or update remote repo using your SSH configurations:
```bash
git remote set-url origin git@github.com-account1:shariful-w3/DevOps1Jenkins.git
```

---

## 📌 Notes

- Always check `.ssh/config` if clone fails.
- Avoid `System.out.println` in production code.
- Use hooks to enforce standards.
- Prefer SSH for secure and multi-account Git access.

---

## 🔧 Additional Useful Commands

### ✅ Install Java (OpenJDK 17)
To install Java on a Debian/Ubuntu-based system:
```bash
sudo apt update
sudo apt install openjdk-17-jdk
```

### 🚀 Running a Spring Boot Application with External Configuration
```bash
java -jar cismiddleware-0.0.1-SNAPSHOT.jar --spring.config.location=file:/root/cis-backend/executables/conf/
```

### 🔐 SSL Certificate Management with Let's Encrypt and PKCS12
Navigate to your certificate directory:
```bash
cd /etc/letsencrypt/live/example.com/
```

Backup the existing `.p12` certificate:
```bash
mv springboot.p12 springboot.p12_old
```

Generate a new PKCS#12 (.p12) file from PEM certificates:
```bash
openssl pkcs12 -export -in fullchain.pem -inkey privkey.pem -out springboot.p12 -name springboot -CAfile chain.pem -caname root
```
