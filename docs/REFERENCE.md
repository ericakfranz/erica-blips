# Erica Blips settings reference

This page lists every setting, its default value, and how the app behaves.

## Format and size

- **Format:** 1:1 is 1080×1080. 9:16 is 1080×1920.
- **Maximum images:** 20.
- **Padding:** 20px on every side by default. You can set each side. In 9:16, the top and bottom start at 250px to keep text in the safe zone. You can change this.

## Headline and body

- **Headline:** the first sentence (it ends at `.`, `!`, or `?`). It appears on slide 1 only. Weight 900.
- **Body:** all the other text. Weight 400.
- **Default sizes:** body 36px, headline 120px. You can change both.
- **Default fonts:** Montserrat for both.
- **Line height:** headline 0.8 em, body 1.6 em.
- **Space below headline:** 0px by default. Auto-fit includes this space.
- **All caps:** optional, for the headline.
- **Line breaks:** the app keeps your line breaks exactly. If you turn on "Optimize", it reflows the text instead.

## Auto-fit

Auto-fit is on by default. It makes the text as large as possible, with one size for all slides.

- The body text never goes below 34px. If the text does not fit, it moves to more slides.
- The headline and body keep a fixed size ratio, so your manual sizes do not apply.
- The line height does not go below 0.8 em (headline) or 1.2 em (body).
- If the text needs more than 20 images even at 34px, the app packs each slide at the minimum line height and shows a warning.
- If one word is wider than the line at the final size, that slide wraps the word instead of letting it run off the edge.

Turn auto-fit off to use your exact sizes.

## Overflow modes (auto-fit off)

- **Force-split mid-paragraph** (default): fill each slide, then continue on the next.
- **Shrink text to fit:** lower the size, down to the minimum font size (default 18px).
- **Push sentence to next slide:** keep the size, and do not split a sentence across slides when a sentence break fits.

If the text does not fit in 20 images, the app cuts it off and shows a warning.

If you set a very large size, the headline can be taller than one slide. The text starts at the top of the slide and the bottom is cut off.

## Fonts

- 71 fonts, grouped as serif, sans-serif, and monospace.
- Choose the headline font and body font separately.
- **Randomize fonts** picks a random font for each, from the category you choose (Any, Serif, Sans, or Mono). The random choice stays until you press Reroll, change the category, or pick a font yourself.

## Text formatting

Select text and use the toolbar buttons for bold, italic, underline, strikethrough, and alignment (left, center, right, justify). The buttons add tags such as `<bold>…</bold>`. You can also type the tags yourself. Tags can be combined. The app closes any tag you leave open.

An alignment tag changes the alignment for that block only.

## Colors

- White on black, black on white, or custom.
- For custom, choose the text color or the background color. The app picks the other color for the best contrast and shows the contrast ratio.
- Contrast uses the WCAG relative-luminance formula. No library is used.

## Print styles

Plain, Riso, Halftone, Cut paper, Misprint, Blueprint, Notebook, Neon, Sunset, and Terminal. Each style adds its own paper and ink look behind the text. The text size and slide splits do not change.

- **Mix and match:** give each slide its own style, cycle through the styles, or shuffle.
- **Plain extras:** a theme or custom color, and a background image with an opacity slider. These controls hide when no slide uses Plain.

## Export

- **Formats:** lossless WebP (default) or PNG.
- **Download:** one slide, all slides as separate files, or one `.zip`.
- **Render scale:** 2×, 3× (default), or 4×.
- **Same as the preview:** the app records where each line breaks on screen and fixes those breaks in the image, so the file matches the preview.
- **Fonts in the image:** the export embeds the exact font. It reads the `.woff2` file with `fetch()` when the page runs from a website. When the page opens from a folder (`file://`), it reads the `.js` copy in `assets/fontdata/`. If a font cannot load, the export stops with a message instead of saving the wrong font.

## Adding a font

1. Add the `.woff2` file to `assets/fonts/`.
2. Add its rule to `assets/fonts.css`.
3. Add a file `assets/fontdata/<slug>-400.js` that contains:

```js
(window.__BLIPS_FONTS=window.__BLIPS_FONTS||{})["Font Name"]="data:font/woff2;base64,...";
```

4. Add the font name and its category to the `FONTS` list in `index.html`.
5. Add the font's license text to `assets/FONT-LICENSES.txt`.

## Interface

- **Light and dark mode:** a button in the header. It changes the app only, not the images.
- **Saved work:** the app saves your text and all settings in your browser. It restores them after a refresh. The **Reset** button clears them.
- **Phones:** on screens 820px wide or less, the layout stacks. A Preview button at the bottom opens a full-screen preview with download buttons.
- **Speed:** the app waits 350 ms after you stop typing, then updates. Very long text takes longer because auto-fit tests many sizes.
