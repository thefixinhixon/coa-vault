# Cannabis COA Vault — The Greenery Room

A web-based vault for Certificates of Analysis (COAs) for cannabis products.
Upload a COA PDF, catalog it once, and any staff member can find and produce
that COA in seconds — by product, batch/lot, vendor, strain, or barcode.

Built for The Greenery Room (Ruidoso / Alto / Carrizozo, New Mexico).
Runs entirely in the browser — no install, works on the Windows PCs in the stores.

## Live app (GitHub Pages)

Once GitHub Pages is enabled for this repo (Settings → Pages → Deploy from
branch `main`, folder `/`), the app is at:

`https://thefixinhixon.github.io/coa-vault/`

## v1 Prototype — what works

- **Bulk upload** — drag & drop one or many COA PDFs
- **Auto-extract** — reads PDF text in-browser (pdf.js) and pre-fills
  product, batch/lot, lab, THC/CBD, dates, and pass/fail where it can find them.
  The uploader always confirms/corrects before saving.
- **Catalog** — product, brand/vendor, strain, type, batch/lot #, lab,
  sample ID, test date, expiration, total THC %, total CBD %,
  pass/fail for pesticides / heavy metals / microbials, store(s), NMS2S barcode,
  notes, and the PDF itself
- **Duplicate check** — warns if the same Batch + Product is already filed
- **Search & filters** — full-text search, filter by store, product type,
  vendor, and status (Valid / Expiring ≤30 days / Expired)
- **Expiry dashboard** — totals, expiring soon, and expired at a glance
- **Missing-COA check** — upload an NMS2S Current Inventory CSV (same export
  the NM Label Generator uses) and see which in-stock products/barcodes
  have no COA on file
- **Export / Import** — export the catalog as JSON or CSV for backup or to
  publish as the shared catalog; import a catalog JSON to load it
- **View / print** — open the filed PDF in one click

## How multi-user sharing works in v1

The app code is hosted on GitHub Pages. Data sharing has two layers:

1. **On each PC (immediate):** catalog entries and PDFs are stored in that
   browser's local storage / IndexedDB. This works offline and needs no login.
2. **Shared vault (publish):** Export the catalog (`Export JSON`), save it as
   `data/catalog.json` in this repo, and put the PDFs in `coas/`.
   On load, the app fetches `data/catalog.json` and merges any shared
   entries it doesn't already have. Everyone visiting the Pages site then
   sees the same shared vault.

See `docs/HOSTING.md` for the publishing steps and the roadmap to a
one-click “Publish to GitHub” upload (GitHub login in-app) for v2.

> Demo data: `data/catalog.json` ships with clearly-marked SAMPLE entries
> so the team can click through search/filters before filing real COAs.
> Delete / replace it when you start filing real COAs. Do not commit real
> COAs to a public repo if you consider them internal — make the repo
> private or keep real PDFs only in local storage / a private shared folder.

## Files

- `index.html` — the whole app (single file, no build step)
- `data/catalog.json` — shared catalog (sample data in v1)
- `data/sample-inventory.csv` — small demo inventory for the Missing-COA check
- `coas/` — place shared COA PDFs here when publishing (see `coas/README.md`)
- `docs/USER_GUIDE.md` — staff guide: uploading and finding a COA
- `docs/HOSTING.md` — hosting, publishing, privacy, and v2 roadmap

## Local test

Open `index.html` in a browser, or serve it:

```bash
python3 -m http.server 8000
# http://localhost:8000
```

Serving over http is recommended — browsers restrict some file/PDF
features on `file://`.
