# Stubs — marketing site

Two static pages for [Stubs: Concert Tracker](https://apps.apple.com/us/app/stubs-concert-tracker/id6758583124)
(App ID `6758583124`). Hand-written HTML and CSS — no framework, no build step,
no bundler, no npm, no JavaScript. Deploys to GitHub Pages by pushing the
directory.

```
index.html      landing page
privacy.html    privacy policy  (SCAFFOLD — see "Blockers" below)
style.css       single stylesheet; design tokens in block #1
robots.txt      allow all + sitemap pointer
sitemap.xml     both URLs with lastmod
assets/         font, icons, screenshots, OG image, badge
```

Runtime page weight is **144 KB** for `index.html` (budget 200 KB) with **zero
external network requests**.

---

## Blocker — this cannot go live yet

One thing is genuinely missing and it cannot be invented:

**The privacy policy text.** `privacy.html` is a *scaffold*. Every section
heading is in place, but the prose is `[TO BE COMPLETED]`. The Notion source
(`https://app.notion.com/p/3014cd8e70a780158b33fc502eceb6ac`) resolves to a
wiki *container* whose body could not be exported, and the URL is not publicly
readable, so no policy text could be carried over. Paste the real text in, then
delete the `.notice` block and the
`<meta name="robots" content="noindex, follow">` at `privacy.html:14`.

> `privacy.html` currently carries `noindex` deliberately, so a page full of
> `[TO BE COMPLETED]` markers cannot get indexed if it ships early. It is still
> listed in `sitemap.xml` as specified. **Removing that one line is required
> for the page to ever rank.** This is the single easiest thing to forget in
> the whole handoff.

Resolved since the first draft: Apple's official App Store badge is now in
place, and the Instagram URL is set to
`https://www.instagram.com/stubsdiary`.

---

## Before going live

### 1. `REPLACE_DOMAIN` — 8 occurrences, 4 files

Replace the literal string `REPLACE_DOMAIN` with the bare domain (no scheme,
no trailing slash — e.g. `stubs.app`). Every occurrence is already prefixed
with `https://`.

| File | Line | Context |
|---|---|---|
| `index.html` | 10 | `<link rel="canonical" href="https://REPLACE_DOMAIN/">` |
| `index.html` | 22 | `og:url` |
| `index.html` | 23 | `og:image` |
| `index.html` | 32 | `twitter:image` |
| `privacy.html` | 10 | `<link rel="canonical" href="https://REPLACE_DOMAIN/privacy">` |
| `robots.txt` | 4 | `Sitemap:` |
| `sitemap.xml` | 4 | `<loc>` — homepage |
| `sitemap.xml` | 8 | `<loc>` — privacy |

```sh
# from this directory
grep -rl REPLACE_DOMAIN . | xargs sed -i '' 's/REPLACE_DOMAIN/your-domain.com/g'
grep -rn REPLACE_DOMAIN .   # must print nothing
```

### 2. `INSTAGRAM_URL` — ✅ done

Set to `https://www.instagram.com/stubsdiary` in the footer of both
`index.html` and `privacy.html`. No placeholder remains.

### 3. Other pre-launch edits

- `privacy.html:14` — delete the `noindex` meta (see Blockers).
- `privacy.html` — replace every `[TO BE COMPLETED]`, delete the `.notice`
  block, set the **Last updated** date.
- `sitemap.xml` — bump both `<lastmod>` values (currently `2026-07-29`).
- `index.html:148` / `privacy.html:128` — the copyright reads `© 2026 Stubs`.

---

## Assets still needed

Everything else in `assets/` is final and was generated from the real brand
files in this repo — no stock imagery, no redrawn icons.

| File | Status | Required size | Notes |
|---|---|---|---|
| `app-store-badge.svg` | ✅ **final** | viewBox `119.66407 × 40` | Apple's official black "Download on the App Store" badge (`Download_on_the_App_Store_Badge_US-UK_RGB_blk_4SVG_092917`). Rendered at 44 px tall — above Apple's 40 px web minimum — with ≥16 px clear space on all sides, which clears Apple's 1/4-of-height rule. Never redraw or recolour it. |
| `text_logo_white.png` | ✅ final | 400 × 140 | The official logotype, used for the header wordmark (24 px tall) and the ticket's event name (30 px tall). **Resized for web from 3191 × 1115 (65 KB → 15 KB)**; the untouched master lives at `../Stubs-logos/Logo/02. PNG/text_logo_white.png`. Sized by `height` with `width: auto`, so replacing it with a different aspect ratio needs no CSS change. |
| `og-image.png` | ✅ generated, replace if you want art direction | 1200 × 630 | Composed here from the real app icon, the real AgenorNeue face, and the ticket motif. Functional and on-brand, but it is not a designed piece — swap it if you'd rather have one. Note it still shows the wordmark set in AgenorNeue rather than the logotype PNG. |
| `screenshot-1..3.webp` | ✅ final | 445 × 905 | Cropped from `../app_store_screenshots/Screenshot 2026-03-21…png` (device frames kept, grey mockup background replaced with `#121212`). Home / Concerts list / Profile stats. |
| `favicon.ico` | ✅ final | 16, 32, 48 | From `../Stubs-logos/App Icon/Stubs - App Icon - 1024 x 1024.png`. |
| `favicon-32.png` | ✅ final | 32 × 32 | Same source. |
| `apple-touch-icon.png` | ✅ final | 180 × 180 | The owner's own 180×180 export. |
| `icon-192.png` / `icon-512.png` | ✅ final | 192 / 512 | Same source. |
| `AgenorNeue-Regular.woff2` | ✅ final | 13.9 KB | Subset of `../swift/LISTD/Fonts/AgenorNeue-Regular.otf` (Latin + punctuation, 169 glyphs, `font-display: swap`). Regenerate with the command below if the copy needs glyphs outside that range. |

Regenerating the font subset:

```sh
pyftsubset ../swift/LISTD/Fonts/AgenorNeue-Regular.otf \
  --output-file=assets/AgenorNeue-Regular.woff2 --flavor=woff2 \
  --unicodes="U+0020-007E,U+00A0,U+00B7,U+00C0-00FF,U+2013,U+2014,U+2018,U+2019,U+201C,U+201D,U+2022,U+2026" \
  --layout-features="kern,liga,calt" --desubroutinize --no-hinting
```

The header wordmark and the ticket's event name both use the logotype PNG. The
webfont is still required — it sets the `<h1>`, the feature lead-ins, the
"ADMIT ONE" line, and every heading on the privacy page.

---

## Post-launch checklist

- [ ] Verify the domain in [Google Search Console](https://search.google.com/search-console) and submit `https://your-domain/sitemap.xml`
- [ ] Verify the domain in [Bing Webmaster Tools](https://www.bing.com/webmasters) and submit the same sitemap
- [ ] **App Store Connect** → App Privacy → replace the Notion privacy-policy URL with `https://your-domain/privacy`
- [ ] **In the app** → Settings screen → update the privacy-policy link to the same URL
- [ ] Update the [@stubsdiary](https://www.instagram.com/stubsdiary) bio link to the new domain
- [ ] Confirm `/privacy` (no extension) resolves — see below
- [ ] Confirm Safari on iOS shows the Smart App Banner on the homepage
- [ ] Run the [Rich Results Test](https://search.google.com/test/rich-results) against the homepage — the `SoftwareApplication` block should parse clean
- [ ] Check the OG card in a link-preview validator (post the URL into Slack/iMessage)

---

## `/privacy` vs `/privacy.html`

The canonical URL and the sitemap both use the extensionless `/privacy`.
GitHub Pages resolves `/privacy` → `/privacy.html` automatically, so both work
in production.

In-page links use the relative `privacy.html` so the site also works from a
plain local file server, which does *not* do extensionless resolution. Crawlers
follow that link, read `<link rel="canonical" href=".../privacy">`, and index
the clean URL.

If a future host does not resolve `/privacy`, move the file to
`privacy/index.html` — the canonical, sitemap, and relative links keep working
with no other edits.

---

## Preview locally

```sh
cd website
python3 -m http.server 8000
# http://localhost:8000/
```

Note that `python3 -m http.server` serves `/privacy.html` but **not**
`/privacy`. That difference is expected and does not apply to GitHub Pages.

---

## Design notes

**The palette and typeface are the iOS app's, not choices made here.** Colour
values are lifted from `../swift/LISTD/Utils/Constants.swift` and
`../swift/LISTD/StatListRow.swift`:

| Token | Value | Source |
|---|---|---|
| `--bg` | `#121212` | `AppColors.screenBackground` |
| `--surface` | `#1e1e1e` | `StatListRow` card fill |
| `--text` | `#fefdff` | `StatListRow` primary — 18.5:1 on `--bg` |
| `--muted` | `#959595` | `StatListRow` secondary — 6.3:1 on `--bg` |
| `--accent` | `#b4ff13` | `AppColors.accentGreen` |

`--accent` appears in exactly two places: the ticket's "ADMIT ONE" line inside
the CTA, and the keyboard focus ring. It is a highlight, not a theme.

**The ticket stub is the signature element and it is the CTA container.** It is
built entirely in CSS (`style.css` block #7) — the perforation is a dashed
`border-top` and the two semicircular cutouts are `::before` / `::after`
circles filled with `--bg`, half-overlapping each edge. Because the page
background is flat, the outer half is invisible and the notch reads as punched
out. No PNG. It mirrors the app's own `Perforation` view in
`../swift/LISTD/Views/Share/ShareCardView.swift`.

There is one load-in fade, wrapped in
`@media (prefers-reduced-motion: no-preference)` so reduced-motion users get
static content. No scroll animations.

### Future-proofing

- **Design tokens live in one clearly-marked block** at the top of `style.css`
  (`#1 DESIGN TOKENS`). Lift that `:root` wholesale into a future web app.
  Do not fork the values — change them in the app and here, or not at all.
- **These paths are kept free** for a future web app, most likely on a
  subdomain: `/app`, `/terms`, `/support`, `/u/`, `/artist/`, `/venue/`.
- This site stays static and pre-rendered permanently. Even once a web app
  exists it must not become a client-rendered route inside it — being
  crawlable is the entire point.

---

## Verified

- Renders correctly at 375, 768, and 1440 px (checked in headless Chrome).
  Note that headless Chrome clamps its window to 500 px wide; test narrow
  viewports through an iframe or real device emulation, not `--window-size`.
- Every text/background pair is ≥ 4.5:1; most are ≥ 13:1.
- One `<h1>` per page; real `<header>` / `<main>` / `<footer>`; every `<img>`
  has `alt` plus explicit `width`/`height`.
- No `outline: none` anywhere and no `!important`; focus is a 2 px `--accent`
  ring with a 3 px offset.
- `sitemap.xml` is well-formed and uses the correct sitemap namespace; the
  JSON-LD parses and carries no `aggregateRating`.
- Zero external requests. Zero JavaScript.

### Not verified

Lighthouse was not run — no Chrome-driving harness is set up in this repo.
Weight, request count, semantics, contrast, and image dimensions were all
checked by hand and should land the Performance ≥ 95 / Accessibility 100 /
SEO 100 targets, but the actual audit is still worth running once the domain
is live. Expect SEO to flag nothing; expect Accessibility to stay 100 provided
the badge swap keeps the `aria-label` on the wrapping `<a>`.
