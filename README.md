
# Fixing 404 on Page Refresh in Nginx (Single Page Application)

If your Nginx server is showing a 404 error when refreshing a page in a React, Angular, or Vue app, follow these steps to fix it. This usually happens because Nginx cannot route client-side paths properly.

---

## ✅ Step-by-Step Guide

### Step 1: Edit Your Nginx Site Configuration

Open your site configuration file located in `/etc/nginx/sites-available/`:

```bash
sudo nano /etc/nginx/sites-available/your-site-config
```

Replace the default `location / {}` block with the following:

```nginx
location / {
    try_files $uri $uri/ /index.html;
}
```

This ensures that Nginx will serve `index.html` for unknown routes, which is crucial for client-side routing.

---

### Step 2: Create Symlink in sites-enabled (if not already linked)

Check if your site configuration is linked to `sites-enabled`:

```bash
ls -l /etc/nginx/sites-enabled/
```

If it's not linked, create a symbolic link:

```bash
sudo ln -s /etc/nginx/sites-available/your-site-config /etc/nginx/sites-enabled/
```

Replace `your-site-config` with your actual file name.

---

### Step 3: Test Nginx Configuration

Before restarting Nginx, make sure the configuration is valid:

```bash
sudo nginx -t
```

If everything is OK, you will see a success message.

---

### Step 4: Restart Nginx

Apply the changes by restarting Nginx:

```bash
sudo systemctl restart nginx
```

---

### ✅ Done!

Your SPA should now handle page refresh correctly without 404 errors.

---

## Notes

- Make sure your build files (like `index.html`, `main.js`, etc.) are located in the correct `root` directory.
- Double-check file and folder permissions if you're still having issues.
