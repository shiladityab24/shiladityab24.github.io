# Portfolio → GitHub Pages + GoDaddy domain

Static hosting on GitHub Pages (free), with your GoDaddy domain. No server,
no build step — GitHub serves `index.html` and `assets/` directly.

> Replace `shiladityabose.com` with your real domain, and `shiladityab24`
> with your GitHub username if different.

What's in this folder:
- `index.html` — your site
- `assets/` — résumé + the two project design-doc PDFs
- `.nojekyll` — tells GitHub Pages to serve files as-is (skip Jekyll)

---

## 1. Create the repository
Create a new **public** repo named exactly:

    shiladityab24.github.io

Naming it `<your-username>.github.io` makes it your "user site," served at
`https://shiladityab24.github.io` (cleanest for a portfolio).

## 2. Add these files to the repo

**Option A — Git (recommended, keeps the `.nojekyll` dotfile):**
```bash
cd portfolio-github
git init
git add .
git commit -m "Portfolio"
git branch -M main
git remote add origin https://github.com/shiladityab24/shiladityab24.github.io.git
git push -u origin main
```

**Option B — Web UI:** on the repo page → "Add file" → "Upload files" → drag in
`index.html` and the `assets` folder → commit. Then "Add file" → "Create new
file" → name it `.nojekyll` (leave empty) → commit.

## 3. Enable Pages
Settings → Pages. For a `*.github.io` repo it usually auto-publishes. Confirm
Source = "Deploy from a branch", Branch = `main`, folder = `/ (root)`, Save.
Live at `https://shiladityab24.github.io` within a minute or two.

## 4. Set the custom domain on GitHub
Settings → Pages → "Custom domain" → enter `shiladityabose.com` → Save.
This commits a `CNAME` file to the repo. (Put your apex here for a clean URL;
GitHub will auto-redirect `www` to it once DNS below is set — or use `www` if
you prefer that as the canonical.)

## 5. Configure GoDaddy DNS
GoDaddy → My Products → your domain → DNS (Manage DNS).

**First remove conflicts:** delete GoDaddy's default parked `A @` record and any
default `CNAME www`. If you set up Forwarding or Heroku records earlier, remove
those too.

**Apex — four A records** (Name `@`, TTL default):

    A   @   185.199.108.153
    A   @   185.199.109.153
    A   @   185.199.110.153
    A   @   185.199.111.153

**www — one CNAME:**

    CNAME   www   shiladityab24.github.io

**Optional IPv6 — four AAAA records** (Name `@`):

    AAAA  @  2606:50c0:8000::153
    AAAA  @  2606:50c0:8001::153
    AAAA  @  2606:50c0:8002::153
    AAAA  @  2606:50c0:8003::153

## 6. Enable HTTPS
DNS usually propagates in minutes (up to 24h). Back in Settings → Pages, wait
for "DNS check successful," then tick **Enforce HTTPS**. The Let's Encrypt
certificate is provisioned automatically; if the box is greyed out, give it time,
or remove and re-add the custom domain to force the cert to regenerate.

## 7. Verify
- `https://shiladityabose.com` and `https://www.shiladityabose.com` both load,
  one redirecting to the other, with a padlock.
- The résumé button and project doc links work (served from `assets/`).

---

## Updating later
Edit `index.html`, then push (or upload via the web UI) — live in ~1 minute.

**Gotcha:** setting the custom domain in step 4 added a `CNAME` file to the repo.
If you work locally with Git, run `git pull` before your next push so you keep
that file — pushing without it can drop your custom domain.
