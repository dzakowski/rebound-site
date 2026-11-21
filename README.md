# ReboundCongestion.org — static marketing/content site

A medically-reviewed, ad-free guide to getting off rebound-causing nasal sprays
(rhinitis medicamentosa / Afrin, Sinex, Otrivin). Plain static site. No backend,
no database, no build step.

## Stack

- Plain HTML, one shared CSS file, one shared JS file, self-hosted fonts.
- No framework, no bundler, no `package.json`, no build.
- Vercel serves the files as-is from the repo root.

## File map

```
index.html        Home
science.html      The Science (mechanism, primer)
self-check.html   "Am I Hooked?" self-check + the 8 signs
playbook.html     Quit playbook (three methods)
timeline.html     Day-by-day recovery timeline
stories.html      Community stories
brands.html       Spray brand reference
faq.html          FAQ
special.html      Pregnancy, CPAP, kids, older adults, athletes
about.html        Editorial standards
sources.html      Full reference list
404.html          Not-found page (served automatically by Vercel)
blog/index.html   Blog index
blog/*.html       One file per article
blog/feed.xml     RSS feed
robots.txt        Crawl rules (AI crawlers explicitly allowed)
sitemap.xml       Sitemap (every indexable page)
llms.txt          Plain-text site summary for AI assistants
vercel.json       Redirects, cache and security headers
.vercelignore     Keeps uploads/ and this README off the public site
assets/styles.css All styling (fonts are self-hosted in assets/fonts)
assets/site.js    All interactivity
assets/og/        Social share images, one per blog article
uploads/          Research source document. Not deployed.
```

Root pages link to each other with relative `.html` paths. Pages in `blog/`
use root-relative paths (`/science.html`, `/assets/...`), so preview the site
with a local server rather than opening files directly:

```bash
python3 -m http.server 8000
```

Images are WebP with explicit `width`/`height` and `loading="lazy"`. Keep new
images under ~100 KB.

## Adding a blog article

1. Copy an existing file in `blog/` and rename it (the filename is the URL).
2. Update, in the `<head>`: `<title>`, meta description, canonical, the
   `og:`/`twitter:` tags, the `article:` dates, and the JSON-LD block
   (headline, description, dates, citations, FAQ questions).
3. Update the body: breadcrumb, `<h1>`, intro, the "short answer" box, the
   sections and table of contents, the FAQ, and the sources list.
4. Add a card for it at the top of `blog/index.html`, an `<item>` in
   `blog/feed.xml`, a `<url>` in `sitemap.xml`, and a line in `llms.txt`.
5. Link to it from at least two related pages.

Only label an article "medically reviewed" once a named clinician has
actually reviewed it (see `about.html`).

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

## Domain

Canonical, Open Graph, sitemap and schema URLs are hardcoded to
`https://www.reboundcongestion.org`. The bare domain redirects to `www`.

## Notes

- No environment variables. No secrets. Nothing to configure.
- Newsletter / Donate / "Send the playbook" buttons are front-end only; wire them
  to a real email/form provider if you want them functional.
