# Image Extension Converter GUI

A lightweight GUI to **batch-convert images to another file extension**.  

Built on Pillow, supports **PNG / JPEG / WEBP / AVIF / HEIC / TIFF / BMP / ICO** (AVIF/HEIC require optional plugins).

---

## Features

- 🖱️ GUI-only workflow (no drag & drop required)
- 🗂️ Folder recursion & pattern filter (e.g., `*.png;*.jpg`)
- 🧰 Minimal inline options per format (controls enable/disable automatically)
- 🧠 EXIF auto-rotate on load + EXIF preserve on save (JPEG/TIFF/HEIC)
- 🫥 Safe transparency handling (alpha-incompatible targets get white background)
- 🧵 Parallel jobs for faster conversion
- 🔌 Optional plugins: `pillow-heif` (HEIC/HEIF), `pillow-avif-plugin` (AVIF)

---

## Supported formats & options

| Output | Main options | Notes |
|---|---|---|
| PNG | `optimize`, `compress_level` | Lossless; higher compression = slower |
| JPEG/JPG | `quality`, `progressive`, `subsampling` | Alpha is composited to white |
| WEBP | `lossless` or `quality`, `method` | `quality` ignored if `lossless=true` |
| AVIF | `quality` | Needs `pillow-avif-plugin` |
| HEIC/HEIF | `quality` | Needs `pillow-heif` |
| TIFF | `compression` | Simplified: alpha composited to white |
| BMP | (none) | Alpha composited to white |
| ICO | `sizes` (e.g., `16 32 48 64`) | Multiple sizes in one file |

---

## Requirements

- Python 3.8+
- Required: `Pillow`
- GUI: **either** `FreeSimpleGUI` **or** `PySimpleGUI`
- Optional:
  - `pillow-heif` (HEIC/HEIF output)
  - `pillow-avif-plugin` (AVIF output)

```bash
# minimal
pip install Pillow FreeSimpleGUI
# or
# pip install Pillow PySimpleGUI<5

# optional format plugins
pip install pillow-heif
pip install pillow-avif-plugin
```

## Usage

```python
python image_ext_converter_gui.py
```

Choose a single image or a folder

Folder: pattern filter with ; separator and recursive option (*.png;*.jpg;*.webp)

Choose target extension, output dir (empty = alongside originals), and overwrite if needed

Tune format options at the bottom (only relevant controls are enabled)

Click Convert to start; progress & logs appear in the window

## FAQ

Q. My transparent PNG became white on JPEG.

JPEG can’t store alpha; the app composites to a white background.

Change the color by editing flatten_alpha() (the bg=(255,255,255) tuple).

Q. Will EXIF be preserved?

Yes for JPEG/TIFF/HEIC when present. Images are auto-rotated on load via ImageOps.exif_transpose().

Q. I can’t pick AVIF/HEIC.

Install the plugins: pillow-avif-plugin (AVIF), pillow-heif (HEIC/HEIF).

Q. Some saves fail due to unsupported params.

The saver retries while skipping unsupported keys automatically. If it still fails, open an issue with the log.

## Screenshots (optional)

docs/screenshot_main.png (main UI)

docs/screenshot_options_jpeg.png (JPEG options)

docs/screenshot_options_png.png (PNG options)

## Project layout
```
.
├─ image_ext_converter_gui.py
├─ README.md
├─ README_ja.md
└─ docs/
   ├─ screenshot_main.png
   ├─ screenshot_options_jpeg.png
   └─ screenshot_options_png.png
```

## Caveats

On PySimpleGUI v4, Multiline has no placeholder_text (not used by this app).

High compression (PNG compress_level=9, WEBP method=6) increases processing time.

TIFF handling is simplified (alpha composited to white).

## License

MIT License


## Contributing

- Issues and PRs are welcome. Ideas:
- More presets per format (high-quality/compact, photo/illustration)

- Background color picker for alpha compositing
- Retry/skip workflow for failed files
- CSV conversion report

## Changelog

v0.1.0 — Initial release (batch GUI, inline per-format options, EXIF auto-rotate/preserve, parallel jobs)




