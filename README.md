# PDPA Egress Inspector — publishing package

Everything needed to put this on the Chrome Web Store. Start with `03-submission-checklist.md`.

```
upload/
  pdpa-egress-inspector-v1.0.0.zip   ← this is the file you upload. Nothing else.
extension/                            ← the same files, unpacked, for Load unpacked and for editing
verify/                               ← run this before every upload
  README.md                           the ten-minute verification procedure
  detector.test.js                    54 automated checks, no dependencies
  test-page.html                      a local page exercising all five channels
  samples/                            synthetic files for the upload channel
store-assets/                         ← screenshots and promo tiles, uploaded in the dashboard
docs/
  00-README.md                        this file
  01-store-listing.md                 every text field, ready to paste
  02-permission-justifications.md     the review fields, plus the answers to likely pushback
  03-submission-checklist.md          the order to do things in
  privacy-policy.html                 host this and paste its URL into the listing
```

## The extension

| File | Role |
| --- | --- |
| `manifest.json` | MV3. Three permissions: `storage`, `scripting`, `activeTab`. No static host permissions. |
| `pdpa-detector.js` | The detection engine. Pure functions, no DOM, no network. |
| `content/inspector.js` | Hooks paste, typing, copy, file and submit on one page. Masks values before anything leaves the tab. |
| `background/service-worker.js` | Registers the inspector per site, keeps the masked log, drives the badge. |
| `popup/` | Today's exposure, add or remove the current site, policy switch. |
| `options/` | Sites, policy, internal hostnames, the full log, JSON and CSV export. |
| `icons/` | 16, 32, 48 and 128 px. |
| `ui.css` | Shared styling for popup and options. |

Four design decisions worth keeping when you edit it:

**No site is inspected until the user adds it.** The manifest declares no content scripts at all.
`chrome.scripting.registerContentScripts` runs per origin, after `chrome.permissions.request`
succeeds for that one origin, and `unregisterContentScripts` runs when the site is removed. This
is what makes an extension that reads typing reviewable at all — do not swap it for a static
`<all_urls>` match to save a few lines.

**Masking happens in the tab.** `content/inspector.js` calls `mask()` before it builds the message
to the service worker. Nothing downstream ever sees a raw NIC, so the log, the export and the
popup are all safe to show someone who should not see the underlying records.

**Permissions are requested from the popup and the options page, never from the service worker.**
`chrome.permissions.request` only works during a user gesture, and a service worker never has one.
The click handler asks Chrome for the origin, and only then messages the worker to register the
script. Moving that call back into the worker looks tidier and silently breaks the entire add-a-site
flow.

**No network code exists in the package.** Not disabled, absent. `chrome.runtime.sendMessage` is
process-to-process messaging inside the browser profile, not a request. Keep it that way; it is
the single strongest answer to every review question you will get.

## Store assets

| File | Where it goes |
| --- | --- |
| `screenshot-1` … `screenshot-5` (1280×800) | Screenshots, in this order. The first is the one most people see. |
| `promo-small-440x280.png` | Small promo tile. Required for the listing. |
| `promo-marquee-1400x560.png` | Marquee tile. Optional, but it is the only asset shown if you are ever featured. |
| `mark-512.png` | Source mark, for a README, a landing page or a favicon. |

The screenshots are rendered from the real interface copy, so if you change a label in the
extension the screenshot will drift. Regenerating them is a Pillow script, not a browser capture.

## Changing detection

`pdpa-detector.js` is the same engine as the standalone `pdpa-dlp-analyzer.html` console from the
earlier build. Tune a pattern in one and copy it to the other, or your demo stops matching your
product. Each detector is an object with `scan(text)` returning `{value, index, confidence}`; add
one by pushing to the `DETECTORS` array, and it flows through scoring, the log and the export
without any other change.

Two things to be careful about when you add patterns: anything that matches a bare run of digits
needs a context gate, or the log fills with invoice numbers within a day; and anything touching
ethnicity, religion or political opinion should stay label-gated, because a detector that fires on
community names in free text is both useless and offensive.

## Before every upload

```
node verify/detector.test.js
```

Then work through `verify/README.md`. It takes ten minutes and it is the difference between finding
a bug yourself and finding out from a reviewer two weeks later.

## Version bumps

Chrome requires a higher `version` in `manifest.json` for every upload. Bump it, rezip the
`extension/` folder contents (manifest at the zip root, not inside a folder), and upload. The
`upload/` zip in this package is built exactly that way if you need a reference.
