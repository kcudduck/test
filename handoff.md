great — here’s a **single handoff doc** you can drop into your project root (e.g. `~/Documents/ice_collapse/handoff_liberator.md`).

---

# 🧊 ICE Collapse Project — PDF Liberation Pipeline Handoff

## 🎯 Current Goal

Build a **PDF “liberator” pipeline** that:

* Extracts clean **page-level PNGs** (no micro-tiles).
* Extracts embedded **text layer** → `<paperId>.text.md`.
* Runs **OCR on every page** → `ocr_pageNN.txt`.
* Places deterministic outputs under
  `~/Documents/ice_collapse/liberated/<domain>/<domain>_pN/`.

---

## 📂 Project Layout

### 1. Source Code (`liberator/`)

```
~/Documents/ice_collapse/liberator/
├── package.json              # pinned deps, "type": "module"
├── node_modules/             # npm deps (pdfjs-dist, sharp, canvas, tesseract.js)
├── beast_mode_wrapper.js     # entry point
├── pdf_liberator_core.js     # pipeline (text, pages, OCR)
├── pdf_utils.js              # rendering utils
├── lb.fish                   # single-PDF wrapper
└── lbb.fish                  # batch wrapper
```

### 2. Domain Outputs (`liberated/`)

Each research domain has its own folder (`d11`, `test`, …).
Each liberated paper → subfolder `<domain>_pN`.

```
~/Documents/ice_collapse/liberated/
├── d10/
│   ├── d10_p1/
│   └── ...
├── d11/
│   ├── d11_p1/
│   └── ...
└── test/
    ├── test_p1/
    │   ├── test_p1.text.md
    │   ├── test_p1.page01.png
    │   ├── ...
    │   ├── ocr_page01.txt
    │   └── ...
    └── test_p2/
        └── ...
```

### 3. Input PDFs (`papers/`)

```
~/Documents/ice_collapse/papers/d11_theoretical_frameworks/
   ├── d11.p1.Some Paper.pdf
   ├── d11.p2.A simple model of ultrasound propagation in a cavitating liquid. Part I.pdf
   └── ...
```

---

## ⚙️ Usage

### Single PDF

```fish
mkdir -p ~/Documents/ice_collapse/liberated/test
lb test ~/Documents/ice_collapse/papers/d11_theoretical_frameworks/'d11.p2.A simple model of ultrasound propagation in a cavitating liquid. Part I.pdf'
```

### Batch

```fish
lbb test ~/Documents/ice_collapse/papers/d11_theoretical_frameworks
```

---

## ✅ Expected Output

* In `~/Documents/ice_collapse/liberated/test/test_p1/`:

  * `test_p1.text.md`
  * `test_p1.page01.png … test_p1.pageNN.png`
  * `ocr_page01.txt … ocr_pageNN.txt`

Console summary:

```
================ LIBERATION SUMMARY ================
📝 Text file:      test_p1/test_p1.text.md
🖼️  Page images:   13 (of 13 pages)
🔎 OCR files:      13 (one per page)
✅ Done.
```

---

## 🔧 Known Issues & Next Steps

* **Fixed**: `pdfjs-dist` import quirks → using CJS default + destructuring.
* **Fixed**: font warnings → `standardFontDataUrl` set.
* **Remaining**: occasional OCR/render warnings on some PDFs — need testing across batches.
* **Future**: add **robust figure cropping** (not just page OCR) without reintroducing tile spam.

---

👉 To continue:
Ask the assistant to **troubleshoot rendering/OCR stability** across a variety of PDFs, then to extend the pipeline with **figure cropping + OCR** once page-level is rock solid.

---

Would you like me to save this straight into your project as `handoff_liberator.md` so you don’t have to copy-paste it manually?

