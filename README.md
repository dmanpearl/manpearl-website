# manpearl.com

Personal website for David Manpearl. Static single-page site hosted on Railway.

---

## Local development

```
python3 -m http.server 8080
```

Open http://localhost:8080 in your browser. No install or build step needed.

---

## Deploy to Railway

### Step 1 - Create a new Railway project

1. Go to https://railway.app and sign in.
2. Click **New Project**.
3. Choose **Deploy from GitHub repo**.
4. Select the `manpearl-website` repository.
5. Railway auto-detects it as a static site (no config file needed).
6. Deployment runs automatically. A temporary Railway domain is assigned (e.g. `manpearl-website-production.up.railway.app`).

### Step 2 - Add the custom domain in Railway

1. In Railway, open your project and click the **Settings** tab.
2. Under **Domains**, click **Add Custom Domain**.
3. Enter `manpearl.com` and click **Add**.
4. Railway shows you a CNAME target and a TXT verification record. Keep this page open.

> Note: Railway also supports `www.manpearl.com`. Add it as a second custom domain if you want both to work. Railway will redirect one to the other.

---

## Configure DNS at OpenSRS (Tucows)

Log in to your OpenSRS reseller panel (or the Tucows DNS management interface for manpearl.com).

### Records to add or update

Google Workspace MX records are already in place - do not change or delete them.

#### For the apex domain (manpearl.com)

Railway provides an A record IP for apex domains. In the Railway domain settings panel, look for **"Apex domain"** instructions. They will show one or more IP addresses.

Add an **A record**:

| Type | Host/Name | Value | TTL |
|------|-----------|-------|-----|
| A | @ (or blank) | (IP shown in Railway) | 300 |

If Railway shows multiple IPs, add one A record per IP.

#### For www subdomain (optional but recommended)

Add a **CNAME record**:

| Type | Host/Name | Value | TTL |
|------|-----------|-------|-----|
| CNAME | www | (Railway domain shown in settings, e.g. `manpearl-website-production.up.railway.app`) | 300 |

#### TXT record for domain verification

Railway requires a TXT record to verify ownership before activating SSL.

| Type | Host/Name | Value | TTL |
|------|-----------|-------|-----|
| TXT | @ (or blank) | (verification string shown in Railway) | 300 |

### After adding DNS records

1. DNS propagation takes a few minutes to a few hours. You can check with:
   ```
   dig manpearl.com A
   dig TXT manpearl.com
   ```
2. Once propagated, go back to Railway and click **Verify** next to the domain.
3. Railway auto-provisions an SSL certificate. The site will be live at https://manpearl.com.

---

## Redeploy after changes

Railway redeploys automatically whenever you push to the GitHub repository's default branch. No manual step needed.

---

## Notes

- Email (Google Workspace MX records) is independent of the web hosting A/CNAME records. Changing A records for web hosting does not affect email delivery.
- The site has no backend, no environment variables, and no build step.
- To add `www` redirect: add `www.manpearl.com` as a second Railway custom domain and set Railway to redirect it to the apex (Railway handles this natively).
