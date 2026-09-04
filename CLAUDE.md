# CLAUDE.md — mfarhadhossain.github.io

This is Farhad Hossain's academic portfolio, deployed as a GitHub Pages static site at **https://mfarhadhossain.github.io**.

## Architecture

- **Generated site** (Hugo + Wowchemy): All HTML files are pre-generated output. There is no live build step in this repo.
- The Hugo source lives elsewhere. If the source is available, regenerate HTML there; otherwise edit HTML directly here.
- Deployment is automatic via GitHub Pages on every push to `master`.

## Key Files

| File | Purpose |
|------|---------|
| `index.html` | Main homepage — the entire live site apart from the two publication pages |
| `uploads/` | PDFs linked from the site (CV, papers) |
| `CV/main.tex` | LaTeX source for the CV. Compile and export to `uploads/CV_FARHAD_HOSSAIN.pdf` |
| `robots.txt` | Search engine crawl directives |
| `sitemap.xml` | SEO sitemap (update when adding/removing public pages) |
| `.nojekyll` | Disables Jekyll processing — the site is pre-generated, nothing to build |
| `_headers`, `_redirects` | ⚠️ Netlify-only formats. **GitHub Pages ignores both.** None of the declared security headers are actually sent. Keep only if a move to Netlify is planned; do not treat as active protection. |

The live site is small: `index.html`, `404.html`, the two `publication/asd-detection/*` pages, and their assets under `css/`, `js/`, `en/js/`, `media/`, `authors/`, `uploads/`. Nothing else is linked. Before deleting anything, confirm it is outside that set.

## Editing Guidelines

- **Adding/removing a publication or project card:** Edit the relevant `<div class="project-card ...">` block in `index.html`. Each card spans roughly 20 lines.
- **Uploading a new PDF:** Add the file to `uploads/` and link it with a relative `href="uploads/filename.pdf"`.
- **Removing a paper from public access:**
  1. Delete the file from `uploads/`.
  2. Remove all `<a href="uploads/filename.pdf">` links in `index.html` (and any other HTML files).
  3. Commit and push. The URL now returns 404.
  4. Request removal in **Google Search Console** → Removals.
  5. **Purge it from git history too** — see below. Deleting a file only removes it from the current tree; the blob stays fetchable from every commit that contained it, and this repo is public.

  **Do NOT add a `Disallow` rule for the removed file.** `Disallow` blocks *crawling*, not indexing. Once Google cannot fetch the URL it can never observe the 404, so the entry can persist in the index indefinitely. Let the URL 404 openly and use Search Console. Reserve `Disallow` for paths that still resolve but should not be discovered.

- **Purging a file from git history:** GitHub serves blobs from any commit it still stores, including commits unreachable from every branch. Check what still reaches the blob:

  ```bash
  for r in $(git for-each-ref --format='%(refname)'); do
    git rev-list --objects "$r" | grep -q "$FILENAME" && echo "REACHES $r"
  done
  ```

  Pull request refs (`refs/remotes/origin/pr/N/merge`) count and are easy to miss — a PR merge ref can hold a commit that exists on no branch. If a PR ref is the only thing holding it, no local `git filter-repo` will help; the PR itself must be deleted, which requires a GitHub Support request.

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
3. Leave the URL crawlable so the 404 is observed — see the warning about `Disallow` above.

## Provenance

This site began as a fork of Dr. Sumon Biswas' Wowchemy site, credited in the `index.html` footer. The fork carried over his generated pages — publications, projects, posts, courses, taxonomy trees — with the author name search-replaced, which left his papers displayed under Farhad's byline on ~16 orphan pages. Those trees were deleted in Aug 2026.

**If the Hugo source is ever rebuilt, check the generated output for inherited content before publishing.** Sweep with:

```bash
grep -ril -e hridesh -e rajan -e biswas -e sumon . --exclude-dir=.git
```

The only expected hit is the footer attribution line in `index.html`.
