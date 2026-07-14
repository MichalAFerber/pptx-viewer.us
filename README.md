# PPTX Viewer

A fast, mobile-first, **single-file** PowerPoint viewer. Drag & drop a `.pptx`
and read the deck **slide by slide** in your browser — text laid out where the
author put it, with images inline. It's a **viewer, not an editor** — no
accounts, no uploads. Everything runs locally in your browser.

🔗 **Live:** <https://pptx-viewer.us/>

![Single file](https://img.shields.io/badge/build-single%20HTML%20file-success) ![No build step](https://img.shields.io/badge/build%20step-none-success) ![License](https://img.shields.io/badge/license-MIT-blue)

> Part of the **[File Viewer](https://file-viewer.us/) family** — HTML, Markdown,
> ePUB, PDF, Data, DOCX, Sheets, EML, and PPTX each have their own dedicated
> viewer. Use the **☰ menu** in the header to jump between them.

## Features

- 🖼️ **Slide-by-slide** — each slide renders at its real aspect ratio with text
  boxes and pictures **positioned where the deck placed them** (EMU coordinates
  mapped into the slide), so layout is preserved, not just content.
- 🔤 **Formatted text** — font size, bold/italic, color, and paragraph alignment
  from the slide are honored; text scales with the slide via container queries.
- 🏞️ **Images inline** — embedded pictures are decoded locally to `data:` URLs.
- 🧭 **Legacy notice** — the old binary `.ppt` has no reliable in-browser
  renderer, so it gets a clear "Save As → .pptx" note.
- ☰ **Family menu** · 🫥 **auto-hiding header** · 🎨 **pick any background color**.
- 🪶 **One file, no build** — `index.html` is self-contained and works offline,
  even from `file://`.
- 📊 **Privacy-friendly analytics** — self-hosted, cookieless
  [Plausible](https://plausible.io/); your decks never leave your device.

## Supported file types

**Rendered:** `.pptx` `.pptm` `.ppsx` `.ppsm` `.potx` `.potm`

**Accepted with a notice** (older binary format): `.ppt`

## Quick start

**Just open it.** Download [`index.html`](index.html), double-click it — no
server, no build, no internet needed.

```sh
python3 -m http.server 8080   # then open http://localhost:8080
```

## Deploy to Cloudflare Pages

Connect the repo (**Workers & Pages → Create → Pages → Connect to Git**),
framework preset **None**, build command blank, output directory `/`. The
[`_headers`](_headers) file applies a strict CSP automatically. Add the custom
domain **pptx-viewer.us** under the project's Custom domains tab.

## How it works

Everything is in [`index.html`](index.html): a small OOXML reader (written for
this project) walks `ppt/presentation.xml` for slide order and size, then each
`ppt/slides/slideN.xml` for shapes — text runs and pictures with their
`a:xfrm` offset/extent — and lays them out as absolutely-positioned boxes scaled
to the slide. [JSZip](https://github.com/Stuk/jszip) is inlined to unzip the
`.pptx` package (a `.pptx` is a ZIP of XML parts). Images inline as `data:`
URLs, so the viewer's CSP stays strict (`default-src 'none'`, no external
assets).

**Note:** this is a viewer — it shows **content + layout**. It is not a
pixel-perfect PowerPoint engine: theme fonts fall back to a system sans-serif,
and advanced features (transitions, animations, SmartArt, charts, embedded
media, master-slide backgrounds) are simplified or omitted.

## Credits

| Component | Version | License |
| --- | --- | --- |
| [JSZip](https://github.com/Stuk/jszip) | 3.10.1 | MIT |
| [pako](https://github.com/nodeca/pako) (bundled in JSZip) | 1.0.x | MIT |

The OOXML slide reader is original to this project. The file-type icon is from
[vscode-icons](https://github.com/vscode-icons/vscode-icons) (MIT). Analytics by
[Plausible](https://plausible.io/).

## License

[MIT](LICENSE) © 2026 Michal Ferber, aka **TechGuyWithABeard**. Bundled
components retain their own licenses — see [`LICENSE`](LICENSE) and
[Credits](#credits).
