# 🌐 DNS Basics: Custom Domain Setup for GitHub Pages and DigitalOcean

## 📘 Overview

The goal of this project is to gain a foundational understanding of **DNS (Domain Name System)** and how to configure domain names to point to:
- A **GitHub Pages site**
- A **DigitalOcean droplet** (or any VPS hosting a static website)

---

## ✅ Prerequisites

- You must own a custom domain name.
  - Purchased from providers like [Cloudflare Registrar](https://www.cloudflare.com/products/registrar/), [Namecheap](https://www.namecheap.com), [GoDaddy](https://www.godaddy.com), etc.
- Completed the following prior projects:
  - ✅ **GitHub Pages site**
  - ✅ **Static site hosted on a remote server using Nginx**

---

## 📁 Project Submission Format

This file includes the **tutorial steps I followed**, screenshots (if applicable), and configuration details I used. This acts as both documentation and reference for future projects.

---

## 🧩 Task 1: Custom Domain for GitHub Pages

### 🛠️ Steps

1. **GitHub Pages Setup**
   - Created a repo: `username.github.io`
   - Added an `index.html` file with content.
   - Enabled GitHub Pages from repository settings → Pages → Source → `main` branch.

   ✅ The site was available at `https://username.github.io`.

2. **DNS Configuration for Custom Domain**
   - My custom domain: `exampledomain.com`
   - Logged in to my DNS provider (e.g., Namecheap, Cloudflare).
   - Added these **DNS records**:

     | Type | Name       | Value                      | TTL  |
     |------|------------|----------------------------|------|
     | A    | @          | `185.199.108.153`          | Auto |
     | A    | @          | `185.199.109.153`          | Auto |
     | A    | @          | `185.199.110.153`          | Auto |
     | A    | @          | `185.199.111.153`          | Auto |
     | CNAME | www       | `username.github.io`       | Auto |

   > These are GitHub's official IP addresses for custom domains.

3. **GitHub Repo Update**
   - Created a file named `CNAME` in the root of the repo:
     ```
     exampledomain.com
     ```
   - Committed and pushed the file.

4. **Verification**
   - Waited for DNS propagation.
   - Navigated to `http://exampledomain.com` and `https://www.exampledomain.com` — ✅ it worked!

---

## 🧩 Task 2: Custom Domain for DigitalOcean Droplet

### 🛠️ Prerequisites
- I already had a working Nginx web server hosting a static site.
- The server's public IP: `123.456.78.90`
- Nginx root directory: `/var/www/html/static-site`

### 🛠️ Steps

1. **DNS Records Setup**
   - Using my domain registrar, I configured the following:

     | Type | Name       | Value           | TTL  |
     |------|------------|------------------|------|
     | A    | @          | `123.456.78.90`  | Auto |
     | A    | www        | `123.456.78.90`  | Auto |

   > This points the domain `exampledomain.com` and `www.exampledomain.com` to the server.

2. **Update Nginx Config**

   On the server, I updated my Nginx config file:

   ```bash
   sudo nano /etc/nginx/sites-available/default
   ```

   Replaced `server_name _;` with:

   ```nginx
   server_name exampledomain.com www.exampledomain.com;
   ```

   Reloaded Nginx:

   ```bash
   sudo systemctl reload nginx
   ```

3. **Verification**

   - Visited `http://exampledomain.com` — ✅ static site loaded.
   - Checked `curl -I http://exampledomain.com` to verify headers.

---

## 🧠 Key Takeaways

- Learned how **A** and **CNAME** records work.
- Configured DNS to point both to:
  - GitHub Pages for version-controlled websites
  - A self-hosted server for full flexibility
- Understood DNS propagation and caching issues.

---

## 🔐 Notes

- DNS changes can take a few minutes to several hours to propagate.
- For HTTPS on GitHub Pages, GitHub automatically provisions an SSL certificate.
- For your own VPS, use Let’s Encrypt with Certbot to enable HTTPS (not covered in this project but recommended).

---

## 📚 Resources

- [GitHub Docs – Custom domains](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site)
- [DigitalOcean DNS Help](https://docs.digitalocean.com/products/networking/dns/)
- [Let’s Encrypt – Certbot](https://certbot.eff.org/)
```

---

https://roadmap.sh/projects/basic-dns
