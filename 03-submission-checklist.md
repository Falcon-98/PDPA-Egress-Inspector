# Submission checklist

## Before you upload

- [ ] **Run `node verify/detector.test.js`.** 54 checks, ten seconds, no dependencies.
- [ ] **Work through `verify/README.md`.** Serve `verify/` over HTTP, load the extension unpacked,
      and follow the seven sections on the test page. Ten minutes. If you skip this you will find
      out about a bug from a reviewer two weeks later instead.
- [ ] **Confirm no raw value reached storage.** Export the log as JSON and search it for
      `923451234V`. Only the masked form may appear.
- [ ] **Replace the three `TODO` values.** The privacy policy needs your email; the listing needs a
      privacy policy URL, a homepage URL and a support URL.
- [ ] **Host `privacy-policy.html`.** GitHub Pages is fine and free. The file is self-contained,
      so dropping it into a `docs/` folder on a public repo and enabling Pages is enough. Chrome
      will not approve an extension that reads user input without a reachable policy URL.
- [ ] **Set `author` in `manifest.json`** if you want a different name shown.
- [ ] **Pay the one-off developer registration fee** if this is your first item. Registration can
      take a day to clear, so do it before you are ready to ship rather than after.

## Uploading

1. Chrome Web Store Developer Dashboard → **Add new item**.
2. Upload `pdpa-egress-inspector-v1.0.0.zip` from the `upload/` folder. That zip contains the
   extension and nothing else — the manifest sits at its root, which is what Chrome requires.
   Do not upload the outer package zip.
3. Store listing tab: paste from `01-store-listing.md`.
4. Upload from `store-assets/`:
   - the five `screenshot-*.png` files, in numbered order, first one first
   - `promo-small-440x280.png` as the small promo tile
   - `promo-marquee-1400x560.png` as the marquee tile (optional, but it is the only asset shown if
     you are ever featured, and it costs nothing to include)
5. Privacy tab: paste the single purpose statement and each permission justification from
   `02-permission-justifications.md`, then tick the three certifications.
6. Distribution tab: Free, all regions, **Unlisted** for the first release.
7. Submit.

## What review will probably ask about

This extension reads what the user types, which puts it in a category reviewers look at closely.
Two things make that survivable, and both are already true of this build — do not weaken either
to ship faster:

- **No static host permissions.** The inspector is registered per site at runtime after the user
  grants that origin. A reviewer can install it and confirm it runs nowhere by default.
- **No network code at all.** There is no `fetch` and no `XMLHttpRequest` anywhere in the package.
  Say so in the justification; it is the fastest way to close the "where does the data go"
  question.

Expect somewhere between a few days and two weeks. Extensions touching user input are routinely
pulled for manual review, and the first submission of a new developer account is slower than
later updates.

If it is rejected, the notice names a specific policy section. The two you are most likely to see:

- *Limited use of user data* — answer by pointing at the masking and the absence of network calls.
- *Requesting unnecessary permissions* — answer with the optional-origins design. The fallback, if
  they insist, is a fixed optional-origin list of common egress destinations plus a managed policy
  for enterprise hosts. Make the argument in `02-permission-justifications.md` first.

## After it is live

- [ ] Run it on **monitor** at one real organisation for a fortnight before selling it as a
      blocking control. You need the false-positive rate against actual staff traffic, and you
      need the baseline finding counts as the business case.
- [ ] Watch for two specific failure modes: a heavy single-page app where the debounced typing
      check fires more than you expect, and long CSV pastes where the 400,000-character cap
      truncates before the interesting rows.
- [ ] Keep the detection engine in `pdpa-detector.js` as the single source of truth. The
      standalone analyzer from the earlier build uses the same code; if you tune a pattern in one
      and not the other, your demo will stop matching your product.
