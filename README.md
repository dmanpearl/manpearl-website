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

## Configure DNS via Cloudflare

Railway only provides CNAME records (not A records), and the DNS spec forbids CNAME records at an apex domain. OpenSRS does not support CNAME flattening, so Cloudflare is used as the DNS provider. Cloudflare's CNAME flattening handles the apex domain transparently.

### Step 1 - Add manpearl.com to Cloudflare

1. Log in to your existing Cloudflare account.
2. Click **Add a domain**, enter `manpearl.com`, and select the free plan.
3. Cloudflare scans existing DNS records and imports them. Verify that your Google Workspace records are present:

| Type | Name | Value | Priority |
|------|------|-------|----------|
| MX | `manpearl.com` | `smtp.google.com` | 1 |
| TXT | `manpearl.com` | `google-site-verification=...` | - |

Add them manually if missing - do not proceed without these or email will break.

4. Cloudflare gives you two nameserver addresses (e.g. `xxx.ns.cloudflare.com`).

### Step 2 - Point OpenSRS to Cloudflare nameservers

1. Log in to OpenSRS and open the management panel for `manpearl.com`.
2. If the domain is locked, disable the **Domain Lock** (Registrar Lock) first.
3. Update the nameservers to the two Cloudflare addresses from Step 1.
4. You can re-lock the domain after the nameservers are saved.

### Step 3 - Add Railway records in Cloudflare DNS

Add both records Railway provided when you added the custom domain:

| Type | Name | Value | Proxy |
|------|------|-------|-------|
| CNAME | `manpearl.com` | (Railway `.up.railway.app` domain) | DNS only (gray cloud) |
| CNAME | `www` | (Railway `.up.railway.app` domain) | DNS only (gray cloud) |
| TXT | `manpearl.com` | (Railway verification string) | - |

Set proxy status to **DNS only** for both CNAMEs - Railway handles SSL itself and the Cloudflare proxy can interfere.

### Step 4 - Verify in Railway

1. DNS propagation takes a few minutes to a few hours.
2. Go back to Railway custom domain settings and click **Verify** for each domain.
3. Railway auto-provisions SSL. The site will be live at https://manpearl.com and https://www.manpearl.com.

---

## Redeploy after changes

Railway redeploys automatically whenever you push to the GitHub repository's default branch. No manual step needed.

---

## Notes

- Email (Google Workspace MX records) is managed in Cloudflare DNS. Do not delete or modify MX records when making web hosting changes.
- The site has no backend, no environment variables, and no build step.
- Both `manpearl.com` and `www.manpearl.com` are configured as separate Railway custom domains.
