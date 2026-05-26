# Lulu for Mayor — Campaign Site

A single-page campaign site for **Lulu**, candidate for 2026 Middlebury Dog Mayor. Static HTML, no build step, deployable to GitHub Pages in under five minutes.

---

## For the LLM Agent: Deployment Instructions

You are deploying this static site to GitHub Pages with the custom domain **luluformayor.com**. Follow the steps below in order. Run each command from the project root (the directory containing this README).

### Project Structure

```
luluformayor/
├── index.html          # The full site — single HTML file, all CSS inline
├── CNAME               # Custom domain config for GitHub Pages
├── README.md           # This file
└── images/
    ├── lulu-hope.jpg       # Obama-style "HOPE" portrait (hero)
    ├── lulu-summit.jpg     # Mount Mansfield summit photo
    ├── lulu-newspaper.jpg  # Addison Independent "Pet of the Week" feature
    ├── lulu-stylish.jpg    # Red campaign jacket
    ├── lulu-cuddle.jpg     # With another dog
    ├── lulu-german.jpg     # On the trail with her human
    └── lulu-cow.jpg        # Halloween cow costume
```

All image paths in `index.html` are relative (`./images/...`). Do not rename files without updating both.

### Prerequisites

- `git` installed
- `gh` (GitHub CLI) installed and authenticated (`gh auth status` should succeed)
- The user owns the domain **luluformayor.com** and has access to its DNS settings
- A GitHub account (free tier is sufficient — GitHub Pages supports custom domains on free)

If `gh` is not available, fall back to the GitHub web UI for repo creation and Pages settings; the git steps are unchanged.

### Step 1 — Initialize the repository

```bash
git init
git add .
git commit -m "Initial campaign site"
git branch -M main
```

### Step 2 — Create the GitHub repo and push

Public repo (required for GitHub Pages on free tier):

```bash
gh repo create luluformayor --public --source=. --remote=origin --push
```

If `gh` isn't available, create the repo at https://github.com/new manually, then:

```bash
git remote add origin https://github.com/<USERNAME>/luluformayor.git
git push -u origin main
```

### Step 3 — Enable GitHub Pages

```bash
gh api -X POST "repos/{owner}/luluformayor/pages" \
  -f "source[branch]=main" \
  -f "source[path]=/"
```

Or via web UI: **Settings → Pages → Source: Deploy from a branch → Branch: main → /(root) → Save**.

Within 1–2 minutes the site will be live at `https://<USERNAME>.github.io/luluformayor/`. Verify before moving on.

### Step 4 — Configure the custom domain

The `CNAME` file in this repo already contains `luluformayor.com`, so GitHub Pages will pick it up automatically on the first push. If it doesn't, set it via:

```bash
gh api -X PUT "repos/{owner}/luluformayor/pages" -f "cname=luluformayor.com"
```

Then tell the user to add the following DNS records at their domain registrar:

**Apex domain (`luluformayor.com`) — four A records:**

| Type | Name | Value             |
|------|------|-------------------|
| A    | @    | 185.199.108.153   |
| A    | @    | 185.199.109.153   |
| A    | @    | 185.199.110.153   |
| A    | @    | 185.199.111.153   |

**Optional `www` subdomain — CNAME record:**

| Type  | Name | Value                       |
|-------|------|-----------------------------|
| CNAME | www  | `<USERNAME>.github.io.`     |

DNS propagation takes anywhere from a few minutes to 48 hours; most registrars resolve in 10–60 minutes.

### Step 5 — Enable HTTPS

Once DNS resolves, return to **Settings → Pages** and check **"Enforce HTTPS"**. GitHub provisions a Let's Encrypt cert automatically (no action needed beyond the checkbox). If the box is greyed out, DNS hasn't propagated yet — wait and retry.

### Step 6 — Verify

```bash
curl -sI https://luluformayor.com | head -5
```

A `200 OK` (or `301` redirecting to https) means the site is live. Open in a browser to confirm the photos load and the QR code renders.

---

## Updating the Site Later

Edit `index.html` or swap images in `images/`, then:

```bash
git add . && git commit -m "Update content" && git push
```

GitHub Pages rebuilds automatically within 30–60 seconds.

---

## Notes for Humans

- **The QR code** is rendered live from `api.qrserver.com` and currently points to the Homeward Bound fundraising events page. Once Homeward Bound publishes the dedicated 2026 Zeffy voting URL (around June 1), update the `data=` parameter in the `<img class="qr-img">` tag in `index.html`.
- **Voting window:** June 1–15, 2026. Swearing-in: June 18, 2026.
- **All proceeds** from votes (\$5 each) go to Homeward Bound, Addison County's Humane Society.
- **Fonts** are loaded from Google Fonts (Alfa Slab One, Playfair Display, Crimson Pro, Special Elite). No fallback fonts are embedded; an internet connection is required for the intended look.
- **No JavaScript** is required for the site to function. All animations are CSS-only.

---

Paid for by the Committee to Elect Lulu. Licks for All.
