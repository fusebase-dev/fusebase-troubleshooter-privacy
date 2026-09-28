---
title: FuseBase Troubleshooter Privacy Policy
---

# FuseBase Troubleshooter Privacy Policy

Effective date: 27 September 2026

Contact: team@nimbusweb.co

## What the extension does with your data

FuseBase Troubleshooter works on your device. A small extension script is present in every frame of the web pages you open so that screenshot, drawing and recording tools can work. It stays idle and sends nothing until you act.

The extension starts reading a page's console messages, errors, the browser's own warnings about it (blocked content, failed loads, deprecated features), clicks and form activity, and the browser-level details of its network requests, only after you act: when you open the extension on that tab, start a screenshot or recording, start a Debug session, or choose "Reload page and capture from load" on a report. If you choose "Reload page and capture from load" on a report, capture continues in that tab until you create the next report from it, stop it from the extension, or 10 minutes pass, even if the tab moves to another site; the toolbar icon shows a dot while it runs (during a recording it shows REC instead). Apart from "Capture from page load" (below), only then does it run code inside that page's own JavaScript, and only in that tab. Network request and response bodies are read only in Debug and Deep Debug. Each report also records browser and device details such as browser version and user agent, operating system, screen size, processor cores, memory class, network type, language, time zone and the region they imply; the names and IDs of the other extensions you have enabled; the page's performance measurements; and any version facts the page publishes about itself, marked as unverified. A page that tampers with its own JavaScript can hide or fake some of what the extension reads from it. If a page takes over the extension's page script before it starts, or replaces a console method it listens to, the report says so in the cases the extension can detect. When the extension's page script starts late or not at all, for example on a busy computer, the report says from when page events were captured.

You can also turn on "Capture from page load" for one website at a time. You confirm it with a second click, and you can turn it off per site or for all sites in Settings. While it is on, a small listener runs inside that website's pages from the moment they load; in incognito windows it keeps nothing. It only listens for errors, unhandled promise rejections, blocked content, failed resource loads and failed requests; it does not change the page's content or behaviour (it passes what it hears through the page's message channel). What it hears is kept for up to 2 minutes in the tab and in the extension's session storage, and it is erased when you close the tab, leave the site or turn the option off. When you start capturing on that tab, what it heard becomes part of that capture. Apart from the idle script described above, this listener is the only extension code that runs in a web page before you act. The toolbar icon shows a dot on that website's tabs while it is on.

Secrets such as passwords, tokens, keys and cookies are replaced with "[REDACTED]" before anything is stored. This is pattern-based and best effort; review a report before you share it. Email addresses in captured text keep their domain and lose the part before the @ ("jane@example.com" is stored as "***@example.com"). The optional page-source file is a copy of the page's HTML in every mode, with form field values removed, secrets in inline scripts, JSON data and data attributes replaced with "[REDACTED]", and email addresses masked as above; other text the page shows stays, so leave the file out when the page itself shows something private. In Deep Debug the report notes whether cookies and login-like storage keys exist, never their values, and the names (not values) of some app storage keys such as feature flags.

Screenshots and recordings (tab or screen video, and microphone or system audio when you turn them on) are made only when you start them and are kept locally like the rest. Before a screenshot is taken, password fields, card and one-time-code fields that carry the standard autocomplete marks, anything a page marks as private, and payment frames are painted over; email fields are not. This is best effort, so review a screenshot before you share it; the report form says how many fields were masked and, for a visible-area or full-page screenshot, can retake it without masking. Recordings are not masked.

Everything is kept in your browser: settings and your site list in the extension's local storage; the live capture, the "Capture from page load" buffer, running reload captures and open report drafts in its session storage, which the browser clears when it closes; screenshots, recordings, page-source files and report evidence in its IndexedDB database. They stay until you remove the extension. Nothing is sent anywhere. It leaves your device only when you download, copy or share a report yourself. One exception is optional and off by default: if you tick "Include my public IP" on the report form, the extension asks api.ipify.org for your public IP address and adds it to that report; the choice is remembered for later reports until you untick it, and while it is ticked the extension asks api.ipify.org when you open the report form.

## Your controls

- Choose which websites diagnostics may run on in Settings (whitelist and exclusions). Excluded websites stay inactive even when you open the extension on them.
- Debug and Deep Debug are temporary sessions that end on their own.
- Stop a "Reload page and capture from load" capture from the extension's menu on that tab.
- You decide what goes into a report before you download or share it.
- An administrator can turn off "Capture from page load" and the public-IP option with managed policies (`disableCaptureFromLoad`, `disablePublicIpLookup`).

## Chrome Web Store privacy practices

On-device handling counts, so every category the extension reads is declared, even where it is redacted before storage.

| Category | Collected | When |
|---|---|---|
| Website content | Console messages, errors, the page source (sanitized), network request metadata, screenshots, tab or screen video and page audio | Only after a user action on that tab, or on an opted-in site |
| User activity | Clicks and form events; typed values only in Debug and Deep Debug, never from password fields, from card or one-time-code fields with the standard autocomplete marks, or from page-marked fields; other values pass through the secret redactor | Only after a user action on that tab |
| Web history | URLs of the pages you inspect, their route changes, and pages a reload capture follows the tab to | Only after a user action on that tab |
| Location | Public IP address; approximate region and time zone from browser settings | Public IP only when you tick "Include my public IP"; region and time zone in every report |
| Personally identifiable information | Names or other personal details a page shows in captured content or text; email domains (the part before the @ is masked) | Only after a user action on that tab |
| Authentication information | Passwords, tokens, cookies and auth headers are read on your device and replaced with "[REDACTED]" before storage; Deep Debug records only whether cookies and login-like storage keys exist, and the names (not values) of some app storage keys such as feature flags | Only after a user action on that tab |
| Financial and payment information | Payment pages can appear in page content and network metadata; card fields with the standard autocomplete marks and payment frames are painted over in screenshots, and their values are never stored as Debug values | Only after a user action on that tab |
| Personal communications | Microphone narration | Only when you turn the microphone on for a recording |

Installed extensions: the names and IDs of the other extensions you have enabled are part of every report, to spot extensions that block or change page requests; the list stays on your device with the rest of the report.

Certifications: the data is not sold, not used or transferred for purposes unrelated to troubleshooting, and not used to determine creditworthiness. It is not transferred to anyone except the public-IP request you opt into and the reports you share yourself. The extension runs no remote code.

## Technical detail

| Code | Where it runs | When |
|---|---|---|
| `content/content-script.js` and its helpers | Isolated extension context of every page, all frames | Present on page load; idle until you act |
| `content/page-observer.js` | Inside the page's own JavaScript, top frame of the tab you inspect | Injected on demand after you act; stays in the page but inert when capture ends; armed and disarmed by the extension with a per-page key; a page that tampers with its own JavaScript can interfere, and the report says so when detected |
| `content/page-debug-wrappers.js` | Inside the page's own JavaScript, top frame | Debug and Deep Debug only |
| `content/prebuffer-main.js` | Inside the page's own JavaScript, top frame of an opted-in site | Registered only while "Capture from page load" is on for that site |
| `content/prebuffer.js` | Isolated extension context, top frame of an opted-in site | Registered only while "Capture from page load" is on for that site; keeps the 2-minute buffer |

While a tab is inspected, in every mode, the extension wraps the page's console methods (log, info, warn, error, debug) so it can read what they print; each call still reaches the page's own console unchanged, and the originals are put back when capture on that tab ends, unless the page replaced them in the meantime. It does not replace the page's network functions (fetch, XMLHttpRequest) outside Debug and Deep Debug, and it never replaces WebSocket.

In Standard mode, network details come only from the browser's own request records; network events posted from inside the page are discarded.
