# SkillPilot Project - Production-Ready Portfolio Page

A portfolio page prepared for production. The CSS and HTML are minified, the page has a `Cache-Control` meta tag for static resources, and the load time is compared with the browser dev tools.

## Folder Structure

```
portfolio-production/
├── src/                     (readable source files, with comments)
│   ├── index.html
│   └── style.css
├── dist/                    (production version, minified)
│   ├── index.html
│   └── style.min.css
└── README.md
```

Open `dist/index.html` to see the production page. The `src` folder keeps the readable version, so the code can still be edited later.

## Task Steps

| Step | Where it is done |
|---|---|
| 1. Compress CSS and HTML by removing whitespace and comments | `src/` is the readable version. `dist/index.html` and `dist/style.min.css` are the minified versions |
| 2. Add `<meta http-equiv="Cache-Control" content="max-age=31536000">` for static resources | In the `<head>` of `dist/index.html` (and `src/index.html`) |
| 3. Verify improved load time using browser dev tools | The results table and the DevTools steps below |

## Step 1: Minify the CSS and HTML

The files in `dist/` are the files in `src/` with this removed:

- all comments
- line breaks and indentation
- extra spaces, and spaces around `{ } : ;`
- the last semicolon in each CSS rule

Numbers and colors were also shortened where possible (for example `0.95rem` became `.95rem`). The production page loads `style.min.css` instead of `style.css`.

The minified page looks exactly the same as the source page.

## Step 2: Cache-Control meta tag

```html
<meta http-equiv="Cache-Control" content="max-age=31536000">
```

`31536000` seconds is one year (365 days x 24 hours x 60 minutes x 60 seconds). It tells the browser that the static resources can be kept and reused for a year.

**Good to know:** Browsers mainly take caching rules from the HTTP headers sent by the server, and many ignore this meta tag. On a real server, the same rule should also be sent as a header: `Cache-Control: public, max-age=31536000`.

**Updating files later:** Because the browser keeps `style.min.css` for a year, rename it when it changes (for example `style.v2.min.css`) or add a version, such as `style.min.css?v=2`, so visitors get the new file.

## Step 3: Verify the load time with browser dev tools

### How to check it

1. Put the `src` files and the `dist` files in separate folders on a local server (for example XAMPP: `htdocs/src-test/` and `htdocs/dist-test/`).
2. Open the page in Chrome and press **F12**.
3. Open the **Network** tab, tick **Disable cache**, and set throttling to **Fast 3G**.
4. Press **Ctrl + Shift + R** to reload.
5. At the bottom of the Network tab, read the **transferred** size, **DOMContentLoaded**, and **Load** time.
6. Do the same for the `src` page and the `dist` page, and compare.
7. To check caching, untick **Disable cache** and reload. A file served with a cache header shows **(disk cache)** in the Size column.

### Results

**File size**

| File | Source | Minified | Saved |
|---|---|---|---|
| index.html | 4,534 B | 3,150 B | 30.5% |
| CSS | 3,723 B | 2,351 B | 36.9% |
| **Total** | **8,257 B** | **5,501 B** | **33.4%** |
| Total with gzip | 2,935 B | 2,263 B | 22.9% |

**Load test** (Chrome, simulated Fast 3G, cache disabled, median of 12 runs)

| | Source | Minified |
|---|---|---|
| Transferred | 8,630 B | 5,874 B (31.9% less) |
| DOMContentLoaded | 180 ms | 179 ms |
| Load | 355 ms | 345 ms |

The minified page transfers about a third less data and loads slightly faster. The time gain is small because this page is very small and most of the load time is network delay. On larger pages, the same minification saves much more time.

## Other Performance Choices

- No images, fonts, or JavaScript files, so the page needs only two requests (HTML and CSS)
- Inline SVG favicon, so the browser does not make an extra request for `favicon.ico`
- System fonts (Arial) instead of downloaded web fonts
- `<meta name="description">` and a `lang` attribute for search engines and accessibility
- Responsive layout with CSS Grid and Flexbox

## Technologies Used

- HTML5
- CSS3 (CSS variables, Grid, Flexbox, media queries)

