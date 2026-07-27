# CF Device Guide

A static instructional site explaining cystic fibrosis and how to use and care for a
CF airway device. No backend, no build step, no JavaScript.

## Stack

Plain HTML and one CSS file. That is the whole thing.

- **No JavaScript.** Interactive behaviour uses native HTML: `<details>` for the
  expandable clinical-detail panels, and a nav bar that wraps instead of collapsing
  into a hamburger.
- **No external requests.** No CDNs, no web fonts, no analytics, no cookies. The
  favicon is an inline data URI and the diagrams are inline SVG.
- **Theme-aware.** Light and dark via `prefers-color-scheme`.
- **Print stylesheet** on every page, so any section prints or saves to PDF cleanly.

## Files

```
index.html          Page 1 — Home / Quick Start
background.html     Page 2 — Background on Cystic Fibrosis
device.html         Page 3 — How the Device Works       (placeholder)
cleaning.html       Page 4 — Cleaning, Sanitation & Maintenance
resources.html      Page 5 — Regional Resources & Support
downloads.html      Page 6 — Downloads & Patient Toolkit (placeholder)
404.html            Not-found page (Cloudflare Pages serves this automatically)
_headers            Cloudflare Pages security headers + cache policy
assets/css/style.css
assets/img/         (empty — for photographs and illustrations)
```

## Local preview

No tooling required — open `index.html` in a browser. For a closer match to
production (absolute paths, 404 handling), serve the directory:

```bash
python3 -m http.server 8000
```

## Deploying to Cloudflare Pages

1. Push this repo to GitHub.
2. Cloudflare dashboard → **Workers & Pages** → **Create** → **Pages** →
   **Connect to Git**, and pick the repo.
3. Build settings:
   - **Framework preset:** None
   - **Build command:** *(leave empty)*
   - **Build output directory:** `/`
4. Deploy. The site is served at `https://<project-name>.pages.dev`.

Every push to `main` redeploys automatically.

### About `_headers`

Sets a strict Content-Security-Policy that allows only same-origin CSS and
`data:` images — nothing else, no scripts at all. **If JavaScript, web fonts, or
external images are ever added, this file must be updated or those resources will
be blocked.**

## Still to do

- [ ] **Hero banner** on `index.html` — intentionally blank, awaiting copy.
- [ ] **`device.html`** — needs the device make/model and its manufacturer IFU.
- [ ] **`cleaning.html`** — sections marked *device-specific*: removable parts list,
      non-immersible parts, and the real replacement-interval table. The current
      intervals are generic placeholders, not this device's figures.
- [ ] **`resources.html`** — items marked *verify*: named Indian CF clinics with
      current contact details, and Chinese-language patient-facing resources.
- [ ] **`downloads.html`** — printables are a proposed shortlist; none built yet.
- [ ] **Site name** — currently the placeholder "CF Device Guide", used in the
      header, `<title>` of each page, and footer.
- [ ] **Images** — `assets/img/` is empty. The two SVG diagrams in `background.html`
      are original; any photographs or anatomical illustrations must be sourced
      under a licence permitting reuse, with attribution.
- [ ] **Clinical review** — no page on this site has been reviewed by a clinician.

## Content notes

Written for patients and caregivers in plain language, with clinician-level detail
tucked into collapsible `<details>` panels so it is available without cluttering the
main read.

Every page carries a medical disclaimer in the footer stating that the
manufacturer's instructions take precedence where they differ from this site.
