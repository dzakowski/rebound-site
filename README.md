# Rebound.org — static marketing/content site

A medically-reviewed, ad-free guide to getting off rebound-causing nasal sprays
(rhinitis medicamentosa / Afrin, Sinex, Otrivin). Plain static site. No backend,
no database, no build step.

## Stack

- Plain HTML, one shared CSS file, one shared JS file.
- No framework, no bundler, no `package.json`, no build.
- Vercel serves the files as-is from the repo root.

## File map

```
index.html        Home / hero + resource cards
science.html      The Science (mechanism, primer)
self-check.html   "Am I Hooked?" 8-sign self-check
playbook.html     14-day quit playbook
timeline.html     Day-by-day recovery timeline
stories.html      Community stories
brands.html       Spray brand reference
faq.html          FAQ
robots.txt        Crawl rules
sitemap.xml       Sitemap
assets/styles.css All styling
assets/site.js    All interactivity
```

All pages link to each other with plain `.html` paths (e.g. `/science.html`).
There are no images — every "photo/diagram" card is CSS-styled text.

## Deploy via GitHub + Vercel

### 1. Create the GitHub repo

```bash
cd rebound-site
git init
git add .
git commit -m "Initial commit: rebound.org static site"
gh repo create <your-name>/rebound-site --public --source=. --push
# or create the repo in the GitHub UI and:
#   git remote add origin https://github.com/<your-name>/rebound-site.git
#   git push -u origin main
```

### 2. Import into Vercel

1. vercel.com -> Add New -> Project -> Import the GitHub repo.
2. Framework Preset: **Other** (it's plain static).
3. Build Command: leave empty. Output Directory: leave empty (serves root).
4. Deploy.

CLI alternative (from inside `rebound-site`):

```bash
npm i -g vercel
vercel        # preview
vercel --prod # production
```

### 3. Custom domain (optional)

In Vercel -> Project -> Settings -> Domains, add your domain and follow the DNS
records Vercel shows.

## IMPORTANT: update hardcoded URLs before going live

Canonical/OG/sitemap URLs are hardcoded to `https://reboundcongestion.org`.
Find-and-replace to your own domain before launch:

```bash
# preview the matches
grep -rl "reboundcongestion.org" . --include="*.html" --include="*.xml"

# replace (macOS/BSD sed)
grep -rl "reboundcongestion.org" . --include="*.html" --include="*.xml" \
  | xargs sed -i '' 's|https://reboundcongestion.org|https://YOUR-DOMAIN.com|g'
```

Files with hardcoded domain: every `*.html` (canonical + Open Graph tags) and
`sitemap.xml`.

## Notes

- No environment variables. No secrets. Nothing to configure.
- Newsletter / Donate / "Send the playbook" buttons are front-end only; wire them
  to a real email/form provider if you want them functional.
