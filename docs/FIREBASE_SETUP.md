# Firebase Setup Guide — COA Vault backend

This guide sets up the shared backend for the COA Vault so multiple staff
can use one vault together, signing in with Google — **no GitHub accounts
for staff, and no server to run.**

You (the owner) do this setup **once**. It takes about 20–30 minutes,
clicking in a web console. No command line, no coding.

**How the pieces fit together**

| Piece | What it does |
|---|---|
| The app itself | Still a web page, still hosted where it is now (GitHub Pages). Firebase does **not** replace it. |
| Firebase Authentication | Staff sign in with Google. |
| Firestore (database) | The shared COA catalog — one record per COA. |
| Cloud Storage | Where the COA PDF files live. |
| Two rules files in this repo (`firestore.rules`, `storage.rules`) | The locks: who can see and change what. You paste them in, once. |

> Privacy note: set this Firebase project up for your **internal**
> deployment. Your real catalog and real PDFs will live in *your* project,
> under your Google account — never in this public repo.

---

## Step 1 — Create the Firebase project

1. Go to [console.firebase.google.com](https://console.firebase.google.com)
   and sign in with **your own Google account** (the one that should own
   the business's data).
2. Click **Create a project** (or "Add project").
3. Name it something plain, e.g. `coa-vault`. The name is for your eyes
   only; staff never see it.
4. Google Analytics: **optional**. Turning it off is fine and simpler.
   Click through and **Create project**, then wait for it to finish and
   click **Continue**.

## Step 2 — Turn on Google sign-in

1. In the left sidebar: **Build → Authentication → Get started**.
2. Open the **Sign-in method** tab, click **Google**, flip it **Enable**,
   and pick a **support email** (your email — it's shown to users if
   sign-in fails). Click **Save**.

## Step 3 — Tell Firebase where the app lives (Authorized domains)

Sign-in only works from web addresses Firebase knows about.

1. Still in **Authentication**, open the **Settings** tab →
   **Authorized domains**.
2. `localhost` is already there (used for testing on one computer).
3. Click **Add domain** and add the domain your app is actually served
   from — just the domain, no `https://` and no path. For example, if
   your app is served from GitHub Pages, the entry looks like:
   `yourname.github.io` — in other words, **your GitHub Pages domain**.
   If you later put the app on your own website domain, add that domain
   too.
4. Skip this and sign-in fails with an "unauthorized domain" error —
   that's the fix if you ever see it.

## Step 4 — Create the catalog database (Firestore)

1. Left sidebar: **Build → Firestore Database → Create database**.
2. Choose **Start in production mode** (that starts locked-down; you'll
   paste in our rules in the next step, which open exactly what's needed).
   Pick the suggested location and click **Enable**.
3. Open the **Rules** tab at the top of Firestore.
4. Delete what's there and **paste in the entire contents of
   `firestore.rules`** from this repo. Click **Publish**.

   These rules are the heart of the access control: only people on your
   allowlist (Step 7) can read the catalog, viewers only see published
   COAs, and only filers/admins can add or change anything.

## Step 5 — Turn on PDF storage (and the one paid-plan step)

1. Left sidebar: **Build → Storage → Get started**. Accept the defaults.
2. **This step requires the Blaze plan.** Cloud Storage now requires the
   pay-as-you-go "Blaze" plan, which means putting a **card on file**
   (Project overview → ⚙ → Usage and billing, or follow the upgrade
   prompt Storage shows you).
   - **What it actually costs:** storage has an always-free allowance —
     roughly the first **5 GB of storage and 100 GB of downloads per
     month cost $0**. A COA PDF is a fraction of a megabyte, so a small
     business vault (hundreds of COAs, a handful of staff) is expected to
     cost **$0/month** in practice. You are paying for a card-on-file, not
     a subscription.
   - **Protect yourself anyway:** in Google Cloud Billing, set a
     **budget alert at $10/month** (Billing → Budgets & alerts → Create
     budget). If anything ever goes sideways, you get emailed long
     before it costs real money.
3. Open Storage's **Rules** tab, delete what's there, and **paste in the
   entire contents of `storage.rules`** from this repo. Click **Publish**.
   - **If the console refuses those rules** with a compile error (some
     projects can't use the cross-service check it contains): open
     `storage.rules`, find the clearly-marked **FALLBACK** block at the
     bottom, and paste that version instead. The file explains the small
     residual risk of the fallback — read that note before choosing it.

## Step 6 — Register the web app and connect it

This tells the vault page which Firebase project is *yours*.

1. Project overview (the Firebase home screen) → click the **web icon
   `</>`** ("Add app → Web").
2. App nickname: `coa-vault`. **Do not** check "Firebase Hosting" — the
   app stays on GitHub Pages. Click **Register app**.
3. Firebase shows you a block of settings called **firebaseConfig**
   (apiKey, authDomain, projectId, storageBucket, messagingSenderId,
   appId). Copy those values into a new file named **`firebase-config.js`**
   placed **next to `index.html`** in your deployment, shaped like this:

   ```js
   window.FIREBASE_CONFIG = {
     apiKey: "…",
     authDomain: "…",
     projectId: "…",
     storageBucket: "…",
     messagingSenderId: "…",
     appId: "…"
   };
   ```

4. **About those values:** they are project *identifiers*, not passwords —
   every visitor's browser needs them to find your project, and the
   security comes from the rules and the allowlist, not from hiding them.
   That said, **your real config belongs in your private/internal
   deployment, not in a public repo** — keep your `firebase-config.js`
   out of any public copy of this tool.

## Step 7 — The allowlist: who can use the vault (bootstrap)

Access is controlled by a list *inside the database* — no code involved.

1. In **Firestore Database → Data**, click **Start collection**.
   Collection ID: `access`
2. For the first document — **that's you**:
   - **Document ID:** your email address, in lowercase
     (for example `you@example.com`)
   - Add two fields:
     - `role` — type **string** — value `admin`
     - `active` — type **boolean** — value `true`
   - Save.
3. Add each staff member the same way: a document in `access`, ID = their
   email (lowercase), with a `role` and `active: true`.

**What the roles mean**

| Role | Can do |
|---|---|
| `viewer` | Search and print **published** COAs only. |
| `filer` | Everything a viewer can, plus upload COAs, edit entries, upload PDFs, and publish. |
| `admin` | Everything a filer can, plus delete entries and manage this allowlist. |

To **revoke** someone: set their `active` field to `false` (or delete
their document). It takes effect on their next page load.

> Tip: use the email address each person actually signs in to Google
> with — a store's shared Google account works fine as a viewer or filer.

## Step 8 — First run: move your existing catalog in

1. Open the vault app and **sign in with Google** (use the account you
   made admin in Step 7).
2. Use the app's **Import** to load your existing catalog JSON.
   Imported entries land as **drafts** — nobody else can see them yet.
3. For each entry, use **Attach PDF** to add the matching COA file.
4. **Visually verify each entry against its PDF** — product, batch,
   THC/CBD, dates, pass/fail. This human check is a deliberate part of
   the workflow; auto-extracted fields can be misread and should never
   go live unverified.
5. Click **Publish** on each verified entry. From that moment, every
   signed-in allowlisted user sees it, live, on any computer.

After this one-time migration, day-to-day filing is just: upload →
verify → publish.

## Costs & privacy, in one place

- **Expected cost: ~$0/month** at small-business scale (see Step 5),
  with a $10 budget alert as a seatbelt.
- **PDFs are not public links.** Per the rules, a PDF can only be
  fetched by someone signed in *and* on your allowlist.
- **Your data is portable.** Use the app's Export (JSON/CSV) any time
  for a backup or to leave the platform — the catalog format is the
  same one the app has always used.
- Staff accounts are just Google accounts; removing someone from the
  allowlist (Step 7) cuts their vault access even if they're still
  signed in to Google.

## Troubleshooting

| Symptom | Likely cause & fix |
|---|---|
| "Not authorized" / empty vault right after sign-in | No document for that email in the `access` collection, or its `active` field isn't `true`. Check spelling and lowercase (Step 7). |
| Sign-in popup fails with an "unauthorized domain" error | The domain the app is served from isn't in Authentication → Settings → Authorized domains (Step 3). |
| Storage rules won't publish / compile error | Use the FALLBACK block at the bottom of `storage.rules` (Step 5). |
| A new COA doesn't show up for other staff | It's still a **draft**. Open it, verify it, and click **Publish** — viewers only ever see published entries. |
| PDFs won't open, but catalog entries show | Storage isn't enabled, the Blaze upgrade is incomplete, or the storage rules weren't published (Step 5). |
