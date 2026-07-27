# CLAUDE.md

## This is a live production site with no staging

GitHub Pages serves `master` directly at <https://menorat-hamaor.co.il/>. There is
no build step and no staging environment — **anything merged to `master` is live
within a minute**. Work on a branch and open a PR; don't commit to `master`.

Repo: <https://github.com/shohamtal/menorat-hamaor> (default branch `master`).
The remote was renamed from `githubpages`, and this directory was renamed to match.

## Two traps that fail silently

Both of these break the page *without any console error*, so a passing test or a
clean console proves nothing here.

1. **`.carousel` must stay `direction: ltr`.** The document is `dir="rtl"` for the
   Hebrew content, but slick positions its track with LTR float/translate maths.
   Letting the carousel inherit RTL translates every slide off-screen and the book
   cover vanishes — while the DOM still reports the image as visible at
   `opacity: 1`, `width: 600`. **Verify the carousel with a screenshot compared
   against the live site, never from computed styles.**

2. **`.carousel` must keep its fixed `height: 400px`.** Until slick initialises the
   three 400px images stack vertically (~1200px) and then collapse to one 400px
   row, shifting the page up ~800px. That was 0.266 of Cumulative Layout Shift;
   reserving the height makes it 0.

## jQuery and slick are pinned with SRI

A wrong `integrity` hash makes the browser refuse the file silently and the
carousel stops working. If you bump a version, recompute:

```sh
curl -sS <url> | openssl dgst -sha384 -binary | openssl base64 -A
```

Sanity-check against the vendor's published hash where one exists before trusting it.

## Don't reintroduce the deleted drafts

`index-10-12-2023.html`, `index-16-11-2023.html` and `gpt-index2.html` were deleted
deliberately. They were unreferenced duplicates; one shared `index.html`'s exact
`<title>`, and one was a scraped copy of a *sefer.org.il* bookshop page whose
canonical pointed at that shop's product page for a different book. They are in git
history if ever needed.

## Verifying SEO changes

```sh
python3 -m http.server 8000        # serve locally, then Lighthouse / screenshot
```

Compare Lighthouse against the live URL for a real before/after. Current state:
SEO 100, Accessibility 100, CLS 0. The one remaining Lighthouse failure —
image aspect ratio / low resolution — is pre-existing: the covers are stretched to
600×400 from 346×479 and 377×422 sources. Fixing it properly needs
higher-resolution images; `object-fit: contain` would letterbox and change the
design, so leave it unless asked.

## The retired predecessor

An earlier Flask + AngularJS version is gone. Its Bitbucket remote no longer
resolves, so the only copy is `~/repos2/_archive/menorat_hamaor-flask-full.bundle`.
Its history contains a hard-coded `SECRET_KEY` — treat it as compromised.

## Content notes

The page is Hebrew, right-to-left, with vocalized (niqqud) quotations. Preserve the
pointing exactly when editing text; it is the whole point of this edition. Rabbi
names in the PDF links (רצאבי, מחפוד, בסיס, גמליאל, ערוסי) are approbations
(הסכמות) — don't "correct" the transliterations in the filenames.
