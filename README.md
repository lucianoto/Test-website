# CF-ARIA

A static site about cystic fibrosis and the CF-ARIA transdermal patch, with separate
sections for patients and for clinicians. No backend, no build step, no JavaScript.

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

Each page is a directory containing `index.html`, so URLs have no `.html`
extension:

```
index.html                    →  /                    Home
understanding-cf/index.html   →  /understanding-cf/   Understanding Cystic Fibrosis
missed-diagnosis/index.html   →  /missed-diagnosis/   Missed Diagnosis of CF in India
for-patients/index.html       →  /for-patients/       For Patients and Families
for-clinicians/index.html     →  /for-clinicians/     For Clinicians
404.html                      →  404                  Served automatically by GitHub Pages
.nojekyll                                             Skip Jekyll; publish files as-is
assets/css/style.css
assets/img/cf-aria.png                                Logo, used in the hero and footer
```

Requesting `/for-patients` without the trailing slash returns a 301 to
`/for-patients/`. That is GitHub Pages' behaviour for directory URLs and is not
configurable — the canonical URL always ends in a slash.

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

Every page except `404.html` uses **relative** paths — `assets/css/style.css`
and `for-patients/` from the root page, `../assets/css/style.css` and
`../for-patients/` from a subdirectory. Because every page sits exactly one level
deep, `../` is uniform. This survives a repo rename or a move to a custom
domain with no edits.

`404.html` is the exception and uses absolute paths prefixed with
`/Test-website/`. It has to: GitHub Pages serves it for *any* unmatched URL at
any depth, so a relative path would resolve differently depending on what the
visitor mistyped. **If the repo is renamed or a custom domain is added, the
paths in `404.html` are the only ones that must be updated.**

## Still to do

-- [ ] **"How CF-ARIA Works"** — blank on both `for-patients/` and `for-clinicians/`,
      showing "Coming soon". Needs the technical description from the laboratory:
      sensing mechanism, materials, wear protocol, readout.
- [ ] **Regulatory wording** — the site says "not approved for clinical use". If the
      device is under an FDA Investigational Device Exemption, the required label is
      more specific: "CAUTION — Investigational device. Limited by Federal law to
      investigational use." Confirm with whoever handles regulatory at GNuLab.
- [ ] **Clinical review** — no page on this site has been reviewed by a clinician.
      Most important for the Treatment part of `for-patients/`, which a reader might
      act on, and for the Evidence section of `for-clinicians/`.
- [ ] **Clinician wording** — unifying the two development-status callouts dropped a
      sentence the clinician page carried: "Nothing on this page describes a device you
      can currently order or deploy in clinical practice." Decide whether it returns.
- [ ] **Logo file** — `assets/img/cf-aria.png` is 219x168 with no alpha channel, so it
      carries a baked-in white background. Both placements work around that with white
      plates. A transparent PNG or SVG at higher resolution would remove the need and
      stay sharp on retina.
- [ ] **Images** — three reserved slots await files: the hero photograph on
      `index.html` and two on `understanding-cf/`. The SVG diagrams there are original;
      any photographs must be licensed for reuse.
- [ ] **India resources** — still missing named CF clinics with current contact details.
- [ ] **Content overlap** — TB misattribution, mutation spectrum, and genetic panel
      limits are covered on both `missed-diagnosis/` and the Resources part of
      `for-patients/`.
- [ ] **Symbol removal** — a pass to strip logos, emojis, and symbols was started and
      deferred. Outstanding: the lungs emoji favicon on every page, the middle dot in
      the inheritance diagram, and three CSS-drawn marks (hamburger bars, the clinical
      disclosure square, the status-flag dot). The link arrows were removed already.
      Undecided: whether the arrows inside the SVG diagrams count, since they carry
      meaning.
- [ ] **Cleaning guidance** — the dedicated cleaning page was removed early on.
      Recoverable from commit `b01ec36` (`git show b01ec36:cleaning.html`).
 [ ] **Repo name** — still `Test-website`, which appears in the public URL. Renaming
      requires updating the absolute paths in `404.html`.

## Content notes

Written for patients and caregivers in plain language, with clinician-level detail
tucked into collapsible `<details>` panels so it is available without cluttering the
main read.

Every page carries a medical disclaimer in the footer stating that the
manufacturer's instructions take precedence where they differ from this site.
