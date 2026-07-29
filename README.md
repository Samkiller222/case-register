# Case Register — Document Extractor

A single-page tool: upload supporting documents for a case (passport page, employer
letter, appointment/flight confirmation, insurance certificate), and it drafts a
case record matching the fields below for you to review, edit, and log.

Fields extracted: Name, Surname, Gender, Passport number, Date appointment, AIP Date,
Flight Date, Accommodation, Insurance, Insurance Expiry, Skills pass, Job title,
Employer, result, Comments.

No backend. It's a static page (`index.html` + `app.js`) that:
- reads text directly out of text-based PDFs (`pdf.js`, loaded from a CDN),
- sends images (and any PDF page it couldn't read as text) straight to the
  Claude API's vision endpoint for extraction,
- shows you the draft so you can correct anything before it's saved,
- keeps a running case log in the browser (`localStorage`) with CSV export.

## Before you use it

You need your own Anthropic API key (from console.anthropic.com). The page asks
for it once and keeps it in your browser's `localStorage` — it is **never**
written to this repo or sent anywhere except `api.anthropic.com`.

**Important security caveat:** because this is a static page with no server,
the API key lives in your browser and every request is made directly from it.
That's fine for personal/local use, but:
- Don't host this on a shared or public machine without clearing the key after.
- Don't commit a key into the repo, ever.
- If you want this properly multi-user or production-grade later, the key
  should move behind a small serverless proxy instead of living in the browser
  — happy to help with that step when you get there.

Passport numbers and personal case data are sensitive — treat the case log
(and any exported CSV) the same way you'd treat a paper case file.

## Running it locally

Just open `index.html` in a browser. No build step, no install.

## Hosting it on GitHub Pages

1. Create a **new** GitHub repository (don't reuse an existing one) —
   e.g. `case-register`.
2. Push this folder to it (see commands below).
3. In the repo: **Settings → Pages → Source → Deploy from branch → main → / (root)**.
4. GitHub gives you a URL like `https://<your-username>.github.io/case-register/`.

## Pushing this to your GitHub

This folder is already a local git repo with one commit. To push it to a new
repo of your own **without touching any existing repo or data**:

```bash
# 1. Create a new, empty repository on github.com first (no README/license),
#    then copy its URL, e.g.:
#    https://github.com/samkiller222/case-register.git

# 2. From inside this folder:
git remote add origin https://github.com/samkiller222/case-register.git
git branch -M main
git push -u origin main
```

That's it — this only touches the new repo you just created; it never reads
from or writes to any other repository.

## Known limitations (v1)

- Scanned PDFs with no text layer aren't converted to images automatically yet
  — re-save that page as a JPG/PNG and upload it directly instead.
- Extraction quality depends on document clarity — always check the draft
  before saving to the log, especially the passport number and dates.
- The case log lives in one browser's `localStorage` — it won't sync across
  devices. Export to CSV regularly if you want a durable copy.
