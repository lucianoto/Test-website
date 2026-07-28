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
resources.html      Page 4 — Regional Resources & Support
downloads.html      Page 5 — Downloads & Patient Toolkit (placeholder)
404.html            Not-found page (GitHub Pages serves this automatically)
.nojekyll           Tells GitHub Pages to skip Jekyll and publish files as-is
assets/css/style.css
assets/img/         (empty — for photographs and illustrations)
```

## Local preview

No tooling required — open `index.html` in a browser. For a closer match to
production (404 handling), serve the directory:

```bash
python3 -m http.server 8000
```

## Deploying to GitHub Pages

Repo settings → **Pages** → under "Build and deployment" set:

- **Source:** Deploy from a branch
- **Branch:** `main`, folder `/ (root)`

Save, and the first build runs in a minute or two. The site is served at:

```
https://lucianoto.github.io/Test-website/
```

Every push to `main` redeploys automatically. No workflow file is needed —
this is plain static HTML, so the built-in branch deployment handles it.

**The repo must be public** unless the account has GitHub Pro/Team/Enterprise.
Pages for private repos is a paid feature.

### Why `.nojekyll`

GitHub Pages runs Jekyll over the repo by default. Jekyll silently skips files
and folders whose names begin with an underscore, and tries to interpret `{{ }}`
and `{% %}` in page content as template syntax. Neither is wanted here.
`.nojekyll` turns all of that off and publishes the files verbatim.

### Security headers

GitHub Pages does not support custom HTTP response headers — there is no
equivalent of Cloudflare's `_headers` file. As a partial substitute, every page
carries these in its `<head>`:

```html
<meta http-equiv="Content-Security-Policy" content="default-src 'none'; style-src 'self'; img-src 'self' data:; font-src 'self'; base-uri 'none'; form-action 'none'">
<meta name="referrer" content="no-referrer">
```

The CSP allows only same-origin CSS and `data:` images — no scripts at all.
**If JavaScript, web fonts, or external images are ever added, this tag must be
updated in every page or those resources will be blocked.**

What cannot be replicated without real headers: `X-Content-Type-Options`,
`Permissions-Policy`, and `frame-ancestors` (the meta form of CSP ignores it, so
clickjacking protection is lost). GitHub Pages does enforce HTTPS and send HSTS
on `github.io`. For a static site with no scripts, forms, or cookies, the
practical exposure from the missing headers is small — but it is a real
difference from the Cloudflare setup.

### Note on paths

`404.html` uses absolute paths prefixed with `/Test-website/`,
because a GitHub Pages *project* site is served from a subdirectory rather than
the domain root. All other pages use relative paths and need no prefix.
**If the repo is renamed, or a custom domain is added, the paths in `404.html`
must be updated to match.**

## Still to do

- [ ] **Hero banner** on `index.html` — intentionally blank, awaiting copy.
- [ ] **`device.html`** — needs the device make/model and its manufacturer IFU.
- [ ] **Cleaning guidance** — the dedicated cleaning page was removed. Decide where,
      if anywhere, cleaning/disinfection/maintenance content should live. The removed
      page is recoverable from commit `b01ec36` (`git show b01ec36:cleaning.html`).
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
