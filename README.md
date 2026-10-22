# Erica Blips

A fully client-side app that turns text into social-media images (formerly Quote Gen) (1:1 and 9:16),
auto-paginating long text into a carousel. No server, no AI, no data leaves the browser.

## How to host on your domain

This is a static site. Upload the whole folder and serve `index.html`:

- **Your own server / nginx / Apache:** drop the folder in your web root.
- **Netlify / Cloudflare Pages / GitHub Pages:** point them at this folder; no build step.

That's it. Everything (text splitting, layout, WebP/PNG encoding, zipping) runs in
the visitor's browser.

## Favicon

The app icon lives in `assets/` (favicon.ico plus 16/32/192/512 PNGs and an Apple touch icon) and is linked from `index.html`. Replace those files to change the icon.

## Dependencies (all open-source, MIT, no AI)

Vendored locally in `vendor/` so the app never depends on a CDN and works offline:

- **html-to-image 1.11.11** — renders the styled DOM frame to canvas/PNG.
- **JSZip 3.10.1** — bundles images into a `.zip`.
- **FileSaver.js 2.0.5** — triggers downloads cross-browser.

All fonts are bundled locally as woff2 files in `assets/fonts/` (declared in `assets/fonts.css`) for the on-screen preview. The same fonts are also in `assets/fontdata/` as small `.js` files (base64). The export embeds the exact font into each image: it reads the woff2 with `fetch()` when the page is served from a website, and falls back to the `.js` copy when the page is opened straight from a folder (`file://`), where browsers block `fetch()`. If a font still cannot be loaded, the export stops with a message instead of saving an image in the wrong font.

If you add a font: add its woff2 to `assets/fonts/`, its rule to `assets/fonts.css`, and a matching `assets/fontdata/<slug>-400.js` file containing `(window.__BLIPS_FONTS=window.__BLIPS_FONTS||{})["Font Name"]="data:font/woff2;base64,...";`.

WebP lossless encoding uses the browser's native `canvas.toBlob('image/webp', 1.0)`.
Contrast auto-calculation uses the WCAG relative-luminance formula (no library).

## Print styles and extras (October 2026)

- **Print styles:** Plain, Riso, Halftone, Cut paper, Misprint, Blueprint, Notebook, Neon, Sunset, Terminal. Each style brings its own paper and inks; decorations sit behind the text, so text sizing and slide splitting never change.
- **Mix & match:** give each slide its own style (menu under each slide), cycle through styles, or shuffle.
- **Plain extras:** Theme / custom color and a background image with an opacity slider. These hide when no slide uses Plain.
- **Space below H1:** extra gap under the headline; auto-fit includes it.
- **Long words:** if a word is still wider than the line at the final size, that slide wraps it instead of letting it run off the edge.
- **Locked line breaks:** before export, the app reads where each line breaks on screen and fixes those breaks in the image, so text cannot re-wrap differently in the file.
- **Accuracy test:** preview and export were compared pixel by pixel across odd cases (wide/condensed fonts, caps, long words, every alignment, inline tags, emoji, blank lines, 9:16, manual oversize, many slides, every style), both from a website and from `file://`.
- **Report issues:** https://github.com/ericakfranz/erica-blips/issues

## How it maps to the spec

- **Ratios:** 1:1 = 1080x1080, 9:16 = 1080x1920, toggle in Format.
- **Auto carousel:** long text paginates; a small block stays a single image.
- **H1:** first sentence (ends at `.`/`!`/`?`) on slide 1 only, Arial weight 900.
- **Body:** Arial weight 400. Defaults: body 36px, H1 120px, both overridable.
- **Alignment:** left (default), center, right, or justified.
- **Fonts:** 69 Google Fonts grouped into serif / sans-serif / monospace. Set H1 and body fonts independently from the dropdowns, or toggle **Randomize fonts** to pick random fonts for each (independently) from a chosen category (Any / Serif / Sans / Mono). Reroll button regenerates the random pick. Monospace options: JetBrains Mono, Space Mono, IBM Plex Mono, Fira Code. The random selection persists across other edits and only changes when you click Reroll, switch category, or pick a font manually.
- **Formatting toolbar:** select text in the box and click a toolbar button to apply **bold / italic / underline / strikethrough** (nestable & combinable) or **left / center / right / justify** (overrides global alignment for that block). Buttons insert tags like `<bold>…</bold>`; you can also type tags manually. Unclosed tags auto-close gracefully. Hover the (i) icon for the reference.
- **Line height:** independent H1 (default 0.8em) and body (default 1.6em) overrides. Auto-fit floors: H1 0.8em, body 1.2em.
- **H1 all caps:** optional toggle.
- **Line breaks:** preserved exactly unless "Optimize" is on (then reflowed).
- **Auto-fit (on by default):** text grows to fill the slide at one uniform size across all slides; body never shrinks below 34px (it spills to more slides instead). If even 34px needs more than 20 images, it packs per-slide at minimum line-heights and warns. Turn off to use literal Body/H1 sizes with the Advanced overflow modes.
- **App theme:** dark/light toggle in the header (cosmetic; output unaffected).
- **Color:** White/Black, Black/White, or Custom (pick text OR background; the opposite is auto-chosen for best contrast; live contrast ratio shown).
- **Padding:** default 20px uniform; switch to per-side. 9:16 auto-sets 250px top/bottom safe zone (overridable).
- **Preview:** every slide previewed in Arial -- the preview IS the capture target, so output matches exactly.
- **Max images:** capped at 20.
- **Output:** lossless WebP (default) or PNG. Download each slide individually, all slides as separate files, or all in one `.zip`.
- **Crispness:** internal render scale 2x/3x/4x (default 3x) for HDR-sharp text.

### Overflow behavior (Advanced)

- **Force-split mid-paragraph** (default)
- **Shrink text to fit** (down to the minimum font size, default 18px)
- **Push sentence to next slide** (keeps size; never splits a sentence across slides when a sentence boundary fits on the page)

## Notes

- **Responsive / mobile:** on screens ≤820px wide the layout stacks vertically and the action buttons become inline (not cut off). Previews are shown via a **sticky Preview button** pinned to the bottom of the screen — tap it to open a full-screen preview overlay (with download buttons), and a Close button pinned to the bottom of the screen returns you to the exact spot you were editing. The inline side preview is desktop-only. Preview size adapts to the viewport.

- **Light/dark preference** is saved in localStorage and restored on revisit.
- **Autosave / refresh protection:** your text and every setting are saved continuously to localStorage and restored silently if you reload or accidentally refresh. The **↺ Reset** button (top right, next to the dark/light toggle) clears the saved session and returns everything to defaults. Rolling autosave only (most recent state); no named saves.
- **Performance:** pagination uses binary search and a plain-text fast path for measurement; typical content renders in ~30ms. Very long blocks (many paragraphs) take longer because auto-fit searches font sizes; the 350ms typing debounce means this only runs when you pause.

- Min-font-size only takes effect in "shrink" mode.
- If text can't fit in 20 images even at minimum size, output is truncated and you're warned.
