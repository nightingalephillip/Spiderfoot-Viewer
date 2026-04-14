# SpiderFoot OSINT Viewer

A single-file, client-side HTML viewer for [SpiderFoot](https://github.com/smicallef/spiderfoot) scan exports. No server, no dependencies, no data leaves your machine — just open the file in a browser and load your JSON.

![SpiderFoot OSINT Viewer](https://img.shields.io/badge/SpiderFoot-OSINT%20Viewer-00d4ff?style=flat-square&labelColor=0d1117)
![No Dependencies](https://img.shields.io/badge/dependencies-none-2dff7a?style=flat-square&labelColor=0d1117)
![Client Side](https://img.shields.io/badge/client--side-only-a78bfa?style=flat-square&labelColor=0d1117)

---

## Features

- **Load any SpiderFoot JSON export** — drag & drop or file picker
- **All events displayed** — nothing sampled or truncated, every row rendered
- **Grouped by event type** — sorted by count, collapsible sections
- **Sidebar navigation** — jump to any event type instantly
- **+ / − Filters** — include or exclude specific event types from view
- **Global search** — filters across all sections simultaneously
- **Per-section search** — filter within a specific event type
- **Collapse / Expand All** — one click to collapse or expand every section
- **Export CSV** — downloads all currently visible rows as a `.csv` file
- **Export Report** — opens a full dark-themed HTML report in a new tab
- **Back to top button** — appears when scrolling, fixed bottom-right
- **Load new scan / Clear** — swap out data or reset to the landing screen without refreshing

---

## Getting Started

### 1. Export your SpiderFoot scan as JSON

In SpiderFoot, after a scan completes:

1. Open the scan results
2. Click **Export** → select **JSON**
3. Save the `.json` file to your machine

### 2. Open the viewer

Just open `spiderfoot_viewer.html` in any modern browser — no installation, no server needed.

```
# Option A — double-click the file in your file manager

# Option B — open from terminal
open spiderfoot_viewer.html        # macOS
xdg-open spiderfoot_viewer.html   # Linux
start spiderfoot_viewer.html       # Windows
```

### 3. Load your scan

Either **drag and drop** your `.json` file onto the landing screen, or click the drop zone to browse for it.

---

## Interface Overview

```
┌─────────────────────────────────────────────────────────────┐
│  TOPBAR — scan info · global search · export · load · clear │
├─────────────────────────────────────────────────────────────┤
│  SUMMARY STRIP — clickable stat tiles (hostnames, emails…)  │
├──────────────┬──────────────────────────────────────────────┤
│              │  [ Collapse All ]  [ Expand All ]            │
│   SIDEBAR    │                                              │
│              │  ┌─ EVENT_TYPE ──────────────── 1,634 ───┐   │
│  event type  │  │  # │ Data │ Source │ Module │ FP │ TS │   │
│  nav list    │  ├────┼──────┼────────┼────────┼────┼────┤   │
│              │  │    │      │        │        │    │    │   │
│  + include   │  └───────────────────────────────────────┘   │
│  − exclude   │                                              │
│              │  ┌─ NEXT_EVENT_TYPE ──────────────────────┐  │
└──────────────┴──────────────────────────────────────────────┘
```

### Sidebar Filters

Each event type in the sidebar has **+** and **−** buttons that appear on hover:

| Button | Behavior |
|--------|----------|
| **+** (include) | Show **only** the types you've marked. Stack multiple `+` types to include all of them |
| **−** (exclude) | **Hide** that event type from view |
| Both together | Include a set of types while excluding specific ones within that set |

An active filter bar appears at the top of the sidebar showing how many types are included/excluded, with a **✕ Clear** button to reset.

### Event Type Color Coding

| Color | Meaning |
|-------|---------|
| 🔵 Cyan | Standard informational types |
| 🔴 Red | Danger — blacklisted, malicious, open cloud storage |
| 🟡 Amber | Warning — unresolved hosts, potential issues |
| 🟢 Green | Host/domain discovery |
| 🟣 Purple | PII — emails, names, phones, social profiles |

### Long Data Fields

For events with long `data` or `source_data` values (WHOIS records, SSL certificates, raw DNS data etc.), the first line is shown with an **[expand]** link to reveal the full content inline.

### Sections Collapsed by Default

The following noisy event types are collapsed by default to keep the view clean:
`SSL_CERTIFICATE_RAW`, `RAW_RIR_DATA`, `RAW_DNS_RECORDS`, `SEARCH_ENGINE_WEB_CONTENT`, `AFFILIATE_DOMAIN_WHOIS`, `CO_HOSTED_SITE_DOMAIN_WHOIS`, `DOMAIN_WHOIS`

Click the section header or use **Expand All** to open them.

---

## Exporting Results

### Export CSV
Exports all **currently visible** rows (respecting any active filters and search terms) as a `.csv` file with all 8 SpiderFoot fields:

`event_type, data, source_data, module, false_positive, last_seen, scan_name, scan_target`

### Export Report
Opens a full dark-themed HTML report in a new browser tab. The report mirrors the viewer visually — same color coding, same layout, same event type badges — and includes a clickable table of contents at the top. Works with browser print (`Ctrl+P` / `Cmd+P`) for PDF export.

---

## Privacy & Security

- **100% client-side** — your scan data never leaves your browser
- **No external requests** — only Google Fonts is loaded (for typography)
- **No tracking, no analytics, no cookies**
- Safe to use with sensitive OSINT data

---

## Compatibility

Works in any modern browser. Tested in Chrome, Firefox, Edge, and Safari.

---

## SpiderFoot JSON Format

The viewer expects a standard SpiderFoot JSON export — a top-level array of event objects:

```json
[
  {
    "data": "accountrecovery.wendys.com",
    "event_type": "INTERNET_NAME",
    "module": "sfp_dnsresolve",
    "source_data": "wendys.com",
    "false_positive": 0,
    "last_seen": "2026-04-13 10:52:33",
    "scan_name": "my_scan",
    "scan_target": "example.com"
  }
]
```

If the file doesn't match this structure an error message will be shown.

---

## License

MIT — do whatever you want with it.
