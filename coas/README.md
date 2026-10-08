# coas/

When publishing a shared vault, put COA PDFs in this folder and reference
them from `data/catalog.json` with a `filePath` like `coas/BATCH-LOT.pdf`.

Naming convention (keeps files findable without the app):

```
<BatchLot>__<Product-Name-Slug>.pdf
e.g.  SAMPLE-BD-240901__Blue-Dream-Flower-3.5g.pdf
```

In v1 local mode, PDFs are stored in the browser's IndexedDB on that PC
and do not need to be copied here — this folder is for the published,
shared copy only.
