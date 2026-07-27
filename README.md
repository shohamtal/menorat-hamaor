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

### Superseded drafts — do not link to these

`index-10-12-2023.html`, `index-16-11-2023.html` and `gpt-index2.html` are older
drafts kept for reference. They each carry `<meta name="robots" content="noindex,
nofollow">` and a canonical pointing at the home page, and are deliberately left
crawlable so the `noindex` is actually seen and honoured — blocking them in
`robots.txt` instead would leave any already-indexed copies stuck in the index.

An earlier version of this site was a Flask + AngularJS application. It is no
longer deployed and has been retired; this static page replaced it.

## Editing notes

Two things in `index.html` are load-bearing and easy to break:

- The document is `dir="rtl"` for the Hebrew content, but **`.carousel` is pinned
  to `direction: ltr`**. The slick carousel positions its track with LTR
  float/translate maths; letting it inherit RTL pushes every slide off-screen and
  the cover silently disappears.
- **`.carousel` has a fixed `height: 400px`.** Before slick initialises, the three
  400px images stack vertically (~1200px) and then collapse to a single 400px row.
  Reserving the height keeps Cumulative Layout Shift at 0 instead of 0.27.

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
