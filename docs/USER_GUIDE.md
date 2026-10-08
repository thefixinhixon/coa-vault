# COA Vault — Staff Guide

## Finding a COA (inspectors / customers) — 30 seconds

1. Open the COA Vault page.
2. In **Find a COA**, type the product name, the batch/lot number from the
   package, or scan / type the barcode.
3. Click **View** → **Open / Print PDF**.
4. Check the product, batch, and expiration on the PDF match the product
   in hand before handing it over.

Filters help if there are many results: narrow by Store, Type, or Status.

## Filing a new COA (when product is received)

1. Save the vendor/lab COA PDF to the computer (email, download, scan).
2. In **Upload COA PDFs**, drag the PDF in (you can drop several at once).
3. Click **Use this file** for the COA you want to file first.
4. The app auto-fills what it can read from the PDF. **Check every field** —
   especially Batch / Lot #, THC %, test date, and expiration date.
   Auto-reading can misread a lab's layout.
5. Tick every **Store** that received this product, add the invoice or
   delivery date in Notes, and click **Save COA to Vault**.
6. If you dropped several PDFs, repeat for the next **Use this file**.

⚠️ If you see a **duplicate warning**, search the vault for that batch first.
File it only once — update the existing entry (Edit) to add another store
instead of creating a second copy.

**Bulk attach:** in the **Account** section, **Attach PDFs to entries
missing files** matches each selected PDF to a catalog entry by exact file
name — and when several entries were filed from one shared bundle PDF under
the same file name, that one upload attaches to every one of those entries.

## Publishing in bulk — "Publish all Drafts" (filers / admins)

Normally every COA is published one at a time, after someone has visually
verified its details against the PDF (the **Publish** button on each Draft
asks for that check). The **Publish all Drafts** button in the **Account**
section is the exception: it publishes *every* Draft in the vault in one
go. Before anything happens it shows a single confirmation that counts
how many of the Drafts are flagged R&D / not-for-sale, how many have a
FAIL safety panel, and how many have no PDF attached — read those numbers
before you confirm. Every entry is stamped with your email as the
verifier. Use it only when you deliberately intend to publish a whole
batch at once, e.g. an initial bulk load of already-reviewed COAs. For
day-to-day filing, stick to verifying and publishing each COA on its own.

## Status badges

- **Valid** — expiration is more than 30 days out
- **Expires in Xd** (yellow) — 30 days or less — plan to move / clear it
- **Expired** (red) — do not rely on this COA for current stock; check with a manager
- **FAIL** (red) — one of pesticides / heavy metals / microbials failed.
  Do not file-and-forget these — tell a manager the same day.

## Tips

- File the COA the same day the product is received, while the batch on
  the invoice is still in front of you.
- The barcode field is worth filling in — it's the fastest lookup at the counter.
- The Missing-COA check (section 4) is a manager task: run it after a
  delivery against a fresh NMS2S inventory export.

## Managing users (admins)

Sign in as an admin and use the **Manage Users** panel (it only appears for admins):

- **Add a user:** type the email of their Google account (exactly as they sign in with Google), choose a role, click **Add user**. Then send them the vault link — they click **Sign in with Google** and they're in. There is no invite email.
- **Roles:** *viewer* — search & print Published COAs only. *filer* — also upload COAs, verify, and publish. *admin* — everything, plus manage users and delete any entry.
- **Change or revoke access:** change the role in the table, untick **Active** to suspend someone without deleting them, or **Remove** to delete their entry entirely. Changes apply the next time that person loads the vault (a refresh is enough).
- You cannot change your own role, deactivate, or remove yourself — that's deliberate, so an admin can't lock everyone out by accident. The very first admin is created once in the Firebase console (see FIREBASE_SETUP.md); after that, everything happens in this panel.
