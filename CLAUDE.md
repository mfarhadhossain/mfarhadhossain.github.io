# CLAUDE.md — mfarhadhossain.github.io

This is Farhad Hossain's academic portfolio, deployed as a GitHub Pages static site at **https://mfarhadhossain.github.io**.

## Architecture

- **Generated site** (Hugo + Wowchemy): All HTML files are pre-generated output. There is no live build step in this repo.
- The Hugo source lives elsewhere. If the source is available, regenerate HTML there; otherwise edit HTML directly here.
- Deployment is automatic via GitHub Pages on every push to `master`.

## Key Files

| File | Purpose |
|------|---------|
| `index.html` | Main homepage — contains About, Publications, Projects, CV sections |
| `uploads/` | PDFs linked from the site (CV, papers) |
| `robots.txt` | Search engine crawl directives |
| `sitemap.xml` | SEO sitemap (update when adding/removing public pages) |
| `_config.yml` | Minimal Jekyll config (jekyll-sitemap plugin only) |
| `_headers` | Netlify security headers (X-Frame-Options, CSP, HSTS) |

## Editing Guidelines

- **Adding/removing a publication or project card:** Edit the relevant `<div class="project-card ...">` block in `index.html`. Each card spans roughly 20 lines.
- **Uploading a new PDF:** Add the file to `uploads/` and link it with a relative `href="uploads/filename.pdf"`.
- **Removing a paper from public access:**
  1. Delete the file from `uploads/`.
  2. Remove all `<a href="uploads/filename.pdf">` links in `index.html` (and any other HTML files).
  3. Add `Disallow: /uploads/filename.pdf` to `robots.txt`.
  4. Commit and push — GitHub Pages will deploy the change; Google will deindex on next crawl (use Google Search Console to request expedited removal).

## Common Tasks

```bash
# Check for any remaining references to a removed file
grep -r "filename.pdf" .

# Stage a deleted file
git add -u uploads/filename.pdf

# Verify no broken PDF links remain
grep -r 'href="uploads/' index.html
```

## Search Engine Removal (for sensitive papers)

After pushing the changes:
1. Go to **Google Search Console** → Removals → New Request → enter the full URL.
2. For **Google Scholar** specifically, contact scholar-support@google.com with the URL to request expedited removal.
3. The `robots.txt` Disallow rule prevents future re-indexing once the cache expires.
