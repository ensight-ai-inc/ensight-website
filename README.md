# ensight-ai.com

Source for the Ensight-AI website. It is a plain static site: one HTML page, one stylesheet, and no build step or framework. To change the site, edit the files and push. The site is meant to be served by GitHub Pages from the `main` branch.

```
index.html              the whole site (sections: hero, problem, approach, ECGvision-ATTR, pipeline, partners, science, milestones, contact)
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

The domain is registered with **Squarespace Domains** (moved over from Google Domains), so DNS is managed at **Squarespace → Domains → ensight-ai.com → DNS**.

The current records, as of November 2026:

| Purpose | Type | Host | Value |
|---|---|---|---|
| Website (Squarespace) | A | @ | 198.185.159.144, 198.185.159.145, 198.49.23.144, 198.49.23.145 |
| Website (Squarespace) | CNAME | www | ext-cust.squarespace.com |
| Email (Google Workspace) | MX | @ | aspmx.l.google.com (1), alt1/alt2.aspmx.l.google.com (5), alt3/alt4.aspmx.l.google.com (10) |
| Email (Google Workspace) | TXT | @ | v=spf1 include:_spf.google.com ~all |
| Email (Google Workspace) | TXT | google._domainkey | v=DKIM1; k=rsa; p=... |

Delete **only** the five website records: the four Squarespace `A` records and the `www` CNAME. In Squarespace DNS they may appear as the **Squarespace Defaults** preset. Then add:

| Type  | Host | Value |
|-------|------|-------|
| A     | @    | 185.199.108.153 |
| A     | @    | 185.199.109.153 |
| A     | @    | 185.199.110.153 |
| A     | @    | 185.199.111.153 |
| CNAME | www  | rohankhera.github.io |

**Do not touch** the email records: the `MX` records, the `v=spf1` TXT record, and `google._domainkey`. If Squarespace shows a **Google Workspace** preset, leave it as is. Email doesn't use the website records, so it keeps working through the switch with no downtime.

Recommended while you're in DNS: the domain has no DMARC record. Add a TXT record on host `_dmarc` with value `v=DMARC1; p=none`. This improves deliverability, since Gmail and Yahoo increasingly expect DMARC.

### 3. Cancel the Squarespace website plan, but keep the domain

Do this only after the new site loads over HTTPS. Cancel only the **website** subscription. The domain is a separate Squarespace Domains registration, paid through 15 Nov 2027. Keep it on auto-renew.

### 4. Google Search Console

1. Add `ensight-ai.com` as a Domain property. If Google Workspace already verified the domain, this is usually instant.
2. Submit `https://www.ensight-ai.com/sitemap.xml`.
3. Use **Removals** to drop the old placeholder pages `https://www.ensight-ai.com/about` and `https://www.ensight-ai.com/team`.

## Editing notes

- **Regulatory language.** ECGvision-ATTR has FDA Breakthrough Device Designation, which is not clearance. Keep the "investigational / not cleared or approved" disclaimers in the product section and footer. Performance numbers are attributed to the peer-reviewed research models, with citations.
- **Adding a team section.** Copy the `cards-3` block from the "Who we work with" section into a new `<section id="team">` and add a nav link.
- **Logo.** The ECG-mark logo is inline SVG in the header and footer, plus `favicon.svg`. Replace all three if a designed logo becomes available.
