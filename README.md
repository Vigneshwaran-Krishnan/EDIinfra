Here’s a nicely formatted **README.md** file ready to be pasted into your git repo. It covers the entire process plus troubleshooting as you requested:

````markdown
# Deploying a Static Website on Ubuntu Server with Nginx

## Overview

This guide explains how to deploy a simple static website (using `index.html`) on an Ubuntu server with Nginx. It covers domain setup, server configuration, and common issues with solutions.

---

## 1. Server Setup and Access

- Provision an Ubuntu server (20.04/22.04 or similar).
- SSH into the server:

  ```bash
  ssh root@your-server-ip
````

---

## 2. Domain Name Configuration

1. Purchase a domain from any registrar.

2. Set an A record in your domain’s DNS to point to your server IP:

   | Host | Type | Value          | TTL    |
   | ---- | ---- | -------------- | ------ |
   | @    | A    | your-server-ip | 30 min |
   | www  | A    | your-server-ip | 30 min |

3. Wait for DNS propagation (usually minutes to hours).

---

## 3. Install Nginx

```bash
sudo apt update
sudo apt install nginx -y
```

Check if Nginx is running:

```bash
sudo systemctl status nginx
```

---

## 4. Configure Nginx for Your Website

1. Create your website directory:

```bash
sudo mkdir -p /var/www/yourdomain.com/html
sudo chown -R $USER:$USER /var/www/yourdomain.com/html
sudo chmod -R 755 /var/www/yourdomain.com
```

2. Add your `index.html`:

```bash
echo "<h1>Welcome to yourdomain.com</h1>" > /var/www/yourdomain.com/html/index.html
```

3. Create Nginx config file:

```bash
sudo nano /etc/nginx/sites-available/yourdomain.com
```

Paste this:

```nginx
server {
    listen 80;
    listen [::]:80;

    server_name yourdomain.com www.yourdomain.com;

    root /var/www/yourdomain.com/html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

4. Enable the site:

```bash
sudo ln -s /etc/nginx/sites-available/yourdomain.com /etc/nginx/sites-enabled/
```

5. Test and reload Nginx:

```bash
sudo nginx -t
sudo systemctl reload nginx
```

---

## 5. Firewall Configuration (if applicable)

Allow HTTP traffic:

```bash
sudo ufw allow 'Nginx HTTP'
sudo ufw reload
```

---

## 6. Verify

Open a browser and visit:

```
http://yourdomain.com
```

You should see your custom welcome message.

---

## 7. Optional: Enable HTTPS with Let’s Encrypt

```bash
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d yourdomain.com -d www.yourdomain.com
```

Follow prompts to issue and install the SSL certificate.

---

## Common Issues & Troubleshooting

| Issue                          | Cause                                  | Fix                                                       |
| ------------------------------ | -------------------------------------- | --------------------------------------------------------- |
| Nginx config test fails        | Syntax errors in config                | Run `sudo nginx -t`, fix errors, check missing semicolons |
| 404 Not Found or no index.html | Incorrect root path or permissions     | Verify root path and permissions (`chmod 755`)            |
| Domain not resolving           | DNS A record not set or not propagated | Verify DNS settings and wait for propagation              |
| Nginx won’t restart            | Port conflicts or config errors        | Kill conflicting process or fix config (`sudo nginx -t`)  |
| 403 Forbidden                  | File/directory permissions issue       | Ensure correct ownership and permissions of website files |
| SSL certificate issues         | Ports blocked or DNS mismatch          | Open ports 80/443 and verify DNS records                  |
| Duplicate server_name errors   | Conflicting Nginx configs              | Ensure unique server_name directives; remove duplicates   |

---

## Summary

* Domain DNS must point to your server IP.
* Nginx serves website files from `/var/www/yourdomain.com/html`.
* Correct file permissions and Nginx configuration ensure proper serving.
* Use `sudo nginx -t` to test configurations before reload.
* Use Certbot for HTTPS.

---

Feel free to customize domain names and paths as needed.
Happy hosting! 🚀

```

---

Want me to help you commit this or push it to your repo? Or anything else?
```
