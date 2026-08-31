# Chrome Web Store listing — copy and paste

Everything below goes into the Developer Dashboard fields with the matching name.
Nothing here is a placeholder except the two URLs marked `TODO`.

---

## Item name

```
PDPA Egress Inspector
```

## Short description (132 character limit — this is 121)

```
Warns before personal data regulated by Sri Lanka's PDPA is pasted, typed or uploaded to a site. Runs entirely on device.
```

## Category

Primary: **Workflow & Planning**
(Secondary, if offered: Developer Tools. Do not pick "Privacy & Security" — that category attracts
extra scrutiny for products claiming to block threats, which this does not do.)

## Language

English (United Kingdom)

---

## Detailed description

```
PDPA Egress Inspector watches the routes personal data actually takes out of a browser — pasted
into a field, typed into a form, copied out of a screen, attached as a file — and tells you when
what is leaving is regulated under Sri Lanka's Personal Data Protection Act No. 9 of 2022.

Everything runs on your own device. No page text is sent anywhere, matched values are masked
before they are written to the local log, and the extension makes no network requests at all.

WHAT IT RECOGNISES

Sri Lankan identifiers, validated rather than pattern-matched:
• National Identity Card, both the old nine-digit format and the twelve-digit format, with the
  embedded day-of-year field checked so ordinary long numbers do not become false alarms
• Passport and driving licence numbers
• Mobile and landline numbers, checked against allocated Sri Lankan prefixes
• Vehicle registration, Taxpayer Identification Number, EPF and ETF membership numbers
• Sri Lankan postal addresses

Financial data:
• Payment card numbers, confirmed with a Luhn check rather than by shape alone
• Bank account numbers, counted only when banking words appear alongside them

Special categories under Schedule II of the Act, which need a separate condition to process:
• Health data, biometric and genetic data
• Ethnic origin, religious belief, political opinion, trade union membership
• Sexual orientation, criminal records
• Data about a child

Two rules that look at the whole payload rather than single values:
• Bulk record set — five or more of the same identifier moving in one action, which is a table
  rather than a case
• Re-identifiable record — an identifier travelling together with an attribute, at which point
  masking a field further downstream no longer helps

CHOOSE WHAT HAPPENS

Monitor quietly, warn on screen, or block critical categories outright. Blocking cancels the
paste, the drop or the form submission before the data reaches the page.

Destination changes the score. The same NIC is weighted far lower going into a system you have
marked as internal than going into a public AI assistant, a webmail draft or a file share.

IT RUNS ONLY WHERE YOU PUT IT

No site is inspected until you add it, and adding one asks Chrome for access to that one site.
Remove it and the permission is withdrawn immediately. There is no blanket access to your
browsing.

AN AUDIT TRAIL YOU CAN ACTUALLY HAND OVER

Every finding keeps its channel, destination, risk score and the section of the Act it bears on.
Export to JSON or CSV. Because values are masked at the point of detection, the export itself
contains no personal data.

WHAT IT IS NOT

This is a detective control that raises the floor and produces evidence. It is not a sealed
perimeter. Someone determined can disable the extension, use another browser, or photograph the
screen. PDF, Word and Excel uploads are reported by filename and size only — the extension cannot
read them, and it does not pretend otherwise.

Deploying this across a team is itself a processing activity under the Act. Tell staff in writing
first and record your lawful basis. Keeping the log masked is why no raw value ever reaches
storage.

LEGAL POSITION

The Act is No. 9 of 2022, amended by the Personal Data Protection (Amendment) Act No. 22 of 2025,
which removed the fixed commencement deadlines. Parts V, VI, VIII, IX and X are in force and the
Data Protection Authority is operating. The substantive controller and processor obligations
commence on a date the Minister appoints by Gazette. Section references shown against findings
are readiness guidance, not advice that a penalty applies today, and this extension is not a
substitute for legal advice.
```

---

## Single purpose statement

Paste this into the "Single purpose" field. Reviewers read it closely; keep it to one purpose.

```
The single purpose of this extension is to detect personal data regulated by Sri Lanka's Personal
Data Protection Act in text the user pastes, types or uploads on sites the user has explicitly
added, and to warn the user or cancel the action before that data leaves the browser.
```

---

## Privacy practices tab

**Does your item collect or transmit user data?**

Answer: the extension handles user data locally but transmits nothing.

Tick only these data-type boxes, because the extension does read them in memory:

- Personally identifiable information — used only to detect and warn the user on their own device
- Website content — text the user pastes, types or uploads on sites they have added

Then tick all three certifications, which are true of this build:

- I do not sell or transfer user data to third parties, outside of the approved use cases
- I do not use or transfer user data for purposes that are unrelated to my item's single purpose
- I do not use or transfer user data to determine creditworthiness or for lending purposes

**Privacy policy URL:** `TODO` — host `privacy-policy.html` from this package and paste the URL.
GitHub Pages works and costs nothing; the file is self-contained.

---

## Support and homepage URLs

- Homepage URL: `TODO` — your GitHub repo or a project page
- Support URL: `TODO` — the repo's issues page is fine

---

## Distribution

- Visibility: **Unlisted** for the first submission. You can test the install flow and hand the
  link to a pilot customer without the listing being publicly searchable, then flip to Public once
  a real organisation has run it for a fortnight.
- Regions: All regions. Restricting to Sri Lanka would cut off Sri Lankan diaspora staff working
  for the same controllers abroad, who are exactly the cross-border case the Act cares about.
- Pricing: Free.
