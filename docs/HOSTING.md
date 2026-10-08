# Hosting, sharing, privacy, and the v2 roadmap

## Hosting (GitHub Pages)

1. Repo: `thefixinhixon/coa-vault` (this repo)
2. GitHub → Settings → Pages → Source: Deploy from a branch → `main` / `(root)`
3. App URL: `https://thefixinhixon.github.io/coa-vault/`
4. Staff bookmark that URL on each store PC. No install, updates are
   automatic when this repo changes.

## How sharing works in v1 (and its honest limits)

GitHub Pages serves static files only — there is no server database
behind it. So v1 uses two layers (see README):

- **Local:** entries + PDFs live in each browser's localStorage / IndexedDB.
  Filing on one PC does not by itself appear on another PC.
- **Shared / published:** a manager exports the catalog JSON, commits it as
  `data/catalog.json`, and commits the PDFs to `coas/` (setting each
  entry's `filePath`). Every PC loading the site then merges the shared
  catalog. Good for a weekly / per-delivery publish rhythm, not instant sync.

This is a deliberate v1 trade: zero cost, zero new accounts, and it proves
the upload / catalog / search workflow before choosing a sync backend.

## Privacy decision (make it before filing real COAs in the repo)

COAs are often shown to customers and inspectors, but a full vault also
reveals vendors, product mix, and batch flow. Options:

- **Public repo** — simplest Pages hosting; treat everything committed as public.
- **Private repo + Pages** — GitHub Pages from a private repo needs a paid
  plan for the account/organization in many cases; verify current plan terms.
- **Public app, private data** — keep this repo public for the app code only,
  never commit real PDFs/catalog; keep the real shared catalog in a private
  location (e.g. a private repo, Google Drive folder for the team) and use
  Export/Import. This is the safest default with v1.

Recommendation: **public app, private data** until we build v2 sync.

## v2 roadmap (in priority order)

1. **In-app GitHub publish** — staff log in with GitHub in the app and
   “Publish” commits the PDF + catalog entry directly. One shared vault,
   no manager export step. Needs a fine-grained token / GitHub App flow.
2. **Real sample-COA tuning** — feed 5–10 real NM lab PDFs through the
   extractor and add per-lab parsing rules (labs format THC/batch very differently).
3. **QR lookup** — print a small QR per batch (ties into the Label Generator)
   so a scan opens that batch's COA — for staff and, if wanted, customers.
4. **Auto missing-COA report** — auto-load the current inventory CSV
   (same pattern as the Label Generator) instead of a manual upload.
5. **Roles** — viewers (search/print only) vs. filers (upload/edit).

## Data model

One catalog entry per COA PDF / batch. Fields are defined by the form in
`index.html` and the sample entries in `data/catalog.json` — that JSON is
the interchange format for export/import/publishing.
