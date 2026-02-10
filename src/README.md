# Local Word Cloud Tool (DOCX or Ranked CSV)

A single-file, local HTML tool that:
- extracts text from a **DOCX** in your browser,
- generates a **ranked word list** (word → count),
- renders a **word cloud**, and
- exports the result as **CSV**, **SVG**, or **PNG**.

No server required. Everything runs client-side in your browser.

---

## Files

- `wordcloud_tool.html` (the only file you need)

Optional inputs:
- Any `.docx` file (e.g., a list of titles, abstracts, bullets, etc.)
- A ranked `.csv` file with columns: `word,count`

---

## How to run

1. Save the tool as `wordcloud_tool.html`.
2. Open it in a modern browser (Chrome or Edge recommended).
3. Choose one input method:
   - **DOCX → Build list from DOCX**
   - **CSV → Build cloud from CSV**

> Tip: If your organization blocks external CDNs, see **Offline Mode** below.

---

## Using a DOCX (recommended workflow)

### 1) Load DOCX
Click **Load DOCX** and select any `.docx` file.

You can upload *any* DOCX file—Word documents, exported DOCX from Google Docs, etc.  
The tool extracts the document’s text in-browser (no upload to a server).

### 2) Build the word list
Click **Build list from DOCX**.

The tool:
- extracts text,
- normalizes punctuation,
- optionally removes years (e.g., 1987, 2024),
- tokenizes into words,
- applies stopwords and filters,
- counts occurrences,
- sorts by frequency.

### 3) Tune the output (important)
Use these controls:

- **Top N words**  
  Limits the ranked list to the top N words after filtering.  
  (Typical ranges: 50–200)

- **Min word length**  
  Filters out short tokens (e.g., “of”, “in”).  
  Try 3–4 for academic titles.

- **Remove years**  
  Removes tokens like 1987 / 2024 if they appear in the text.

- **Drop numbers-only tokens**  
  Removes pure numeric tokens (still keeps alphanumeric like `SiO2`, `GaAs`).

- **Stopwords** (the most important control)  
  A list of words to ignore, one per line or comma-separated.  
  This includes common English stopwords plus common academic “filler” words.

---

## Editing the word outputs (how to control what appears)

You have **two main ways** to edit the final words in the cloud.

### Method A: Edit stopwords (fastest)
If an unwanted word is dominating the cloud:

1. Find it in the cloud or ranked list.
2. Add it to the **Stopwords** box.
3. Rebuild (click **Build list from DOCX** again).

Examples of common “junk” words to add:
- `study`, `analysis`, `characterization`, `development`, `effects`, `properties`

You can also remove words from stopwords if you *do* want them included.

### Method B: Export CSV → edit → reload (most control)
If you need precise control (e.g., merge synonyms, force counts, remove a few terms):

1. Build from DOCX.
2. Click **Export ranked CSV**.
3. Edit the CSV in Excel (or any spreadsheet tool).
   - You can delete rows, rename words, or change counts.
4. Save as CSV (keep `word,count` columns).
5. Load the edited CSV using **Load Ranked CSV**.
6. Click **Build cloud from CSV**.

This gives full manual control over the final word list.

---

## Exporting graphics

### Export SVG (recommended for design workflows)
Click **Export SVG** to download `wordcloud.svg`.

- Best for Illustrator / InDesign / PowerPoint (vector)
- You can edit text as objects (unless you later outline it)

> Note: The exporter replaces the `system-ui` font keyword with `Helvetica` in the SVG
> to avoid Illustrator import warnings.

### Export PNG
Click **Export PNG** to download `wordcloud.png`.

- Good for quick slide insertion
- The PNG exporter preserves aspect ratio based on the SVG viewBox (no squishing)

If you want a specific slide size (e.g., 1920×1080 or 3840×2160), the PNG export function
can be adjusted in the script.

---

## Tips for best results

- **Start with Top N = 100–150** and **Min length = 3**
- Expand stopwords aggressively to remove “academic glue” words
- If you have chemical formulas/material names:
  - they often appear as alphanumeric tokens (e.g., `GaAs`, `SiO2`) and will be kept
- Consider exporting CSV and merging synonyms:
  - e.g., `magnetic` + `magnetism`, `diffusion` + `diffusive`

---

## Offline mode (no internet access)

By default, the tool loads libraries from CDNs:

- `d3`
- `d3-cloud`
- `mammoth` (DOCX parsing)

If you need a fully offline version:
1. Download those JS files.
2. Place them in the same folder as `wordcloud_tool.html`.
3. Replace the three `<script src="https://...">` tags with local paths, e.g.:

```html
<script src="./d3.min.js"></script>
<script src="./d3.layout.cloud.js"></script>
<script src="./mammoth.browser.min.js"></script>
```

---

## Troubleshooting

### Illustrator warning on import

If Illustrator complains about fonts (e.g., `system-ui`), export SVG again—the tool sanitizes that token to `Helvetica` for compatibility.

### “Squished” PNG exports

PNG export uses the SVG `viewBox` to preserve aspect ratio. If you still see distortion, verify you’re viewing the PNG at 100% and that the viewer isn’t applying non-uniform scaling.

### DOCX content looks incomplete

Some Word docs contain text inside tables/text boxes. Mammoth extracts “raw text” best from normal paragraphs. If your content is in tables/text boxes, consider copying it into a simple paragraph/list format first, or export a ranked CSV manually.

---
