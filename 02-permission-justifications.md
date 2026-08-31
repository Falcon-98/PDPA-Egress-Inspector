# Permission justifications

Each field in the dashboard maps to one block below. Paste them verbatim. Reviewers reject
justifications that restate the permission name instead of describing the feature that needs it.

---

### `storage`

```
Stores the user's own settings (which sites to inspect, whether to monitor, warn or block, and
which hostnames count as internal systems) and a local log of findings so the user can review and
export what was detected. The log holds only masked values, never raw personal data, and it never
leaves the device.
```

### `scripting`

```
The inspector is not declared statically in the manifest. It is registered with
chrome.scripting.registerContentScripts only for the specific sites the user has added, after
Chrome has granted the matching optional host permission, and unregistered the moment the user
removes that site. This is what keeps the extension off every other page the user visits.
```

### `activeTab`

```
The toolbar popup shows the hostname of the current tab and offers a one-click button to start or
stop inspecting it. activeTab supplies that hostname only when the user opens the popup, which
avoids requesting the broader "tabs" permission.
```

### `optional_host_permissions` (`http://*/*`, `https://*/*`)

```
Requested one origin at a time, never as a group. When the user adds a site, the extension calls
chrome.permissions.request for that single origin; when they remove it, chrome.permissions.remove
withdraws it. The wildcard appears in the manifest only because the set of sites an organisation
needs to watch is not knowable in advance — a bank watches its own CRM and its staff's webmail, a
hospital watches something else entirely. The user is never prompted for all sites at once and the
extension never runs on a site that was not explicitly added.
```

### Remote code

```
No remote code. All logic ships in the package. The extension makes no network requests of any
kind and loads no external scripts, fonts or stylesheets.
```

---

## Notes on likely review questions

**"Why does a privacy extension read what I type?"**
Because typing is one of the four channels a leak travels through, and the one an organisation
most often overlooks. Typing inspection is debounced, runs only on user-added sites, skips
password fields entirely, and can be turned off in options while pastes, uploads and submits stay
covered.

**"Does this transmit page content?"**
No. `chrome.runtime.sendMessage` carries findings from the content script to the extension's own
service worker inside the same browser profile. That is process-to-process messaging within the
extension, not a network request. There is no `fetch`, no `XMLHttpRequest`, and no host in the
code other than the pattern lists used to classify a destination.

**"Why `<all_urls>`-shaped optional permissions?"**
See above. If the reviewer pushes back, the fallback is to ship with a fixed match list of the
common egress destinations (`chatgpt.com`, `mail.google.com`, `drive.google.com` and so on) as
optional origins, and let enterprise deployments add their own hosts through a managed policy.
That is a worse product for the customer, so make the argument above first.
