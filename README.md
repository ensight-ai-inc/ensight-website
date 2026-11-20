# ensight-ai.com

Source for the Ensight-AI website. It is a plain static site: one HTML page, one stylesheet, and no build step or framework. To change the site, edit the files and push. The site is meant to be served by GitHub Pages from the `main` branch.

```
index.html              the whole site (sections: hero, problem, approach, ECGvision-TTR, pipeline, partners, science, milestones, contact)
assets/styles.css       all styling
assets/og-image.png     preview image used when the link is shared (LinkedIn, Slack, iMessage)
assets/apple-touch-icon.png
favicon.svg
404.html                shown for unknown URLs, including the old Squarespace /about and /team pages
CNAME                   tells GitHub Pages to serve www.ensight-ai.com
robots.txt, sitemap.xml for search engines
```

## Preview locally

```bash
python3 -m http.server 8000
```

Then open http://localhost:8000.

## Go-live checklist (one time)

### 1. Turn on GitHub Pages

1. Merge the site into `main`.
2. Make the repository public, or upgrade to GitHub Pro. GitHub Pages on a free personal account only works with public repositories. The repository contains only public website content.
3. Go to **Settings → Pages**. Set **Source** to "Deploy from a branch", choose **Branch** `main` and folder `/ (root)`, and save.
4. In the same page, set **Custom domain** to `www.ensight-ai.com`. After DNS resolves, tick **Enforce HTTPS**.
5. Recommended: verify the domain under **Settings (account) → Pages → Verified domains**. This adds one TXT record and prevents domain takeover.

### 2. Point DNS at GitHub Pages

Make these changes wherever the domain's DNS is managed. If the domain was bought through Google Domains or Squarespace, that is **Squarespace → Domains → ensight-ai.com → DNS**.

Remove the Squarespace website records: the `A` records on `@` and the `www` CNAME that point to Squarespace. In Squarespace DNS these appear as the **Squarespace Defaults** preset. Then add:

| Type  | Host | Value |
|-------|------|-------|
| A     | @    | 185.199.108.153 |
| A     | @    | 185.199.109.153 |
| A     | @    | 185.199.110.153 |
| A     | @    | 185.199.111.153 |
| CNAME | www  | rohankhera.github.io |

**Do not touch** the email records: `MX` records (Google), the `TXT` record starting `v=spf1`, `google._domainkey` (DKIM), `_dmarc`, and any `google-site-verification` TXT. Google Workspace email keeps working because those records are unchanged.

### 3. Cancel the Squarespace website plan, but keep the domain

Do this only after the new site is live over HTTPS. If the domain is registered at Squarespace, cancel only the **website** subscription and keep the **domain** registration (about $20/year) on auto-renew.

### 4. Google Search Console

1. Add `ensight-ai.com` as a Domain property. If Google Workspace already verified the domain, this is usually instant.
2. Submit `https://www.ensight-ai.com/sitemap.xml`.
3. Use **Removals** to drop the old placeholder pages `https://www.ensight-ai.com/about` and `https://www.ensight-ai.com/team`.

## Editing notes

- **Regulatory language.** ECGvision-TTR has FDA Breakthrough Device Designation, which is not clearance. Keep the "investigational / not cleared or approved" disclaimers in the product section and footer. Performance numbers are attributed to the peer-reviewed research models, with citations.
- **Adding a team section.** Copy the `cards-3` block from the "Who we work with" section into a new `<section id="team">` and add a nav link.
- **Logo.** The ECG-mark logo is inline SVG in the header and footer, plus `favicon.svg`. Replace all three if a designed logo becomes available.
