# Lulu for Mayor — Campaign Site

A single-page campaign site for **Lulu**, candidate for 2026 Middlebury Dog Mayor. Static HTML, no build step, deployable to GitHub Pages in under five minutes.

---

## For the LLM Agent: Deployment Instructions

You are deploying this static site to GitHub Pages. The public-facing link is a short URL — **https://tinyurl.com/luluformayor** — that redirects to the GitHub Pages site. There is no custom domain. Follow the steps below in order. Run each command from the project root (the directory containing this README).

### Project Structure

```
luluformayor/
├── index.html          # The full site — single HTML file, all CSS inline
├── CNAME               # Unused — gitignored, not deployed (site is shared via TinyURL, not a custom domain)
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
- A GitHub account (free tier is sufficient — GitHub Pages is free for public repos)
- (Optional) A URL shortener (e.g. TinyURL) to create a memorable share link pointing at the Pages URL

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

### Step 4 — Public URL (short link)

This site is **not** served from a custom domain. The canonical Pages URL is:

```
https://germanr.github.io/luluformayor/
```

For sharing — flyers, word of mouth, anywhere a clean link matters — it sits behind a short link:

```
https://tinyurl.com/luluformayor  →  https://germanr.github.io/luluformayor/
```

The `CNAME` file in this repo is **gitignored on purpose** (see `.gitignore`) and is never deployed, so GitHub Pages reports no custom domain — that's expected. If you ever want a real domain instead, un-ignore and commit `CNAME`, then add the four GitHub Pages A records (`185.199.108–111.153`) at your registrar. Until then, no DNS setup is needed; just point a TinyURL at the Pages URL above.

### Step 5 — Enable HTTPS

On a `*.github.io` URL, HTTPS is automatic and always enforced — GitHub provisions the Let's Encrypt cert for you and there's no toggle to flip. Nothing to do here. (The "Enforce HTTPS" checkbox under **Settings → Pages** only matters if you later add a custom domain.)

### Step 6 — Verify

```bash
curl -sI https://germanr.github.io/luluformayor/ | head -5
curl -sIL https://tinyurl.com/luluformayor | grep -iE '^HTTP|^location'
```

A `200 OK` on the Pages URL means the site is live; the TinyURL should `301` to it. Open the short link in a browser to confirm the photos load and the QR code renders.

---

## Updating the Site Later

Edit `index.html` or swap images in `images/`, then:

```bash
git add . && git commit -m "Update content" && git push
```

GitHub Pages rebuilds automatically within 30–60 seconds.

---

## Notes for Humans

- **The QR code** is rendered live from `api.qrserver.com` and points to the Zeffy voting ballot (`zeffy.com/en-US/ticketing/middlebury-dog-mayor-election`). To change the destination, update the `data=` parameter in the `<img class="qr-img">` tag in `index.html`, and the matching `vote-link` href just below it.
- **Voting window:** June 1–15, 2026. Swearing-in: June 18, 2026.
- **All proceeds** from votes (\$5 each) go to Homeward Bound, Addison County's Humane Society.
- **Fonts** are loaded from Google Fonts (Alfa Slab One, Playfair Display, Crimson Pro, Special Elite). No fallback fonts are embedded; an internet connection is required for the intended look.
- **No JavaScript** is required for the site to function. All animations are CSS-only.

---

Paid for by the Committee to Elect Lulu. Licks for All.
