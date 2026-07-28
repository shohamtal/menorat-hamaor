# מנורת המאור — menorat-hamaor.co.il

Static site for the annotated edition of **מנורת המאור** by Rabbi Yitzchak Aboab,
vocalized in Yemenite pointing and explained by **ציון טל (בוטא)**.

Served by **GitHub Pages** from `master` at <https://menorat-hamaor.co.il/>
(domain via the `CNAME` file). There is no build step — `index.html` is served
as-is, so anything committed here goes live.

## Layout

| Path | Purpose |
| --- | --- |
| `index.html` | The entire site: one Hebrew RTL page, inline CSS |
| `style.css` | Legacy stylesheet, largely superseded by the inline styles |
| `images/1–3.png` | Book cover / sample pages shown in the slick carousel |
| `demo/*.pdf` | Sample chapters and rabbinic approbations (הסכמות) |
| `favicon.svg` | Menorah favicon (also used as the apple-touch-icon) |
| `robots.txt`, `sitemap.xml` | Crawl directives; sitemap lists the page + all PDFs |
| `CNAME` | Custom domain for GitHub Pages |
| `google256c1f51882b8dbb.html` | Google Search Console verification |

### Removed drafts

`index-10-12-2023.html`, `index-16-11-2023.html` and `gpt-index2.html` used to sit
in this directory and were served publicly. They have been deleted — nothing linked
to them, and they were duplicates: the first was 97% contained in `index.html` and
shared its exact `<title>` (competing for the same query), the third was fully
contained, and the second was a 773 KB scraped copy of a *sefer.org.il* bookshop
page whose canonical pointed at that shop's product page for a different book.
They now 404, which is the correct signal for removing them from the index.

They are recoverable from git history if ever needed.

### Predecessor

An earlier version of this site was a Flask + AngularJS app in a separate repo. It
is retired and not deployed. Its history is archived at
`~/repos2/_archive/menorat_hamaor-flask-9aff049.bundle` (restore with `git clone`).
That app had a hard-coded `SECRET_KEY` committed to its history — treat it as
compromised and do not reuse it.

## Editing notes

Two things in `index.html` are load-bearing and easy to break:

- The document is `dir="rtl"` for the Hebrew content, but **`.carousel` is pinned
  to `direction: ltr`**. The slick carousel positions its track with LTR
  float/translate maths; letting it inherit RTL pushes every slide off-screen and
  the cover silently disappears.
- **`.carousel` has a fixed `height: 400px`.** Before slick initialises, the three
  images stack vertically and then collapse to a single 400px row. Reserving the
  height keeps Cumulative Layout Shift at 0 instead of 0.27.
- **Each slide is a wrapper `<div class="slide">`, not a bare `<img>`.** Slick writes
  the slide width as an inline style onto the direct children of `.carousel`; with
  bare images it stretched every cover to 600px and distorted the portrait ones.
  Images are sized with `max-height`/`max-width` so they keep their proportions at
  any viewport width.

The jQuery and slick assets are pinned with Subresource Integrity hashes. If you
bump a version you must recompute the hash, or the browser will silently refuse to
load the file and the carousel will stop working:

```sh
curl -sS <url> | openssl dgst -sha384 -binary | openssl base64 -A
```

## Local preview

```sh
python3 -m http.server 8000   # then open http://localhost:8000/
```
