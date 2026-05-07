# AGENTS.md

## Project Overview

Single-page HTML tool for Microsoft channel partners to generate Statement of Work (SOW) documents for **Microsoft Purview Information Protection (MIP) & Data Loss Prevention (DLP)** engagements. Everything lives in one self-contained file: [mip-sow-generator.html](mip-sow-generator.html). Scoped to M365 Business Premium + Purview Suite add-ons.

## Architecture

- **No build system, no dependencies, no frameworks** — pure HTML + CSS + vanilla JS in a single file
- `<style>` block (lines ~8–114) → HTML form + output panels → `<script>` block (lines ~743–878)
- Left panel: 6 form sections (all visible, no step wizard — user scrolls)
- Right panel: generated SOW document with print/copy actions
- Two-column CSS grid: form panel (430px fixed) + output panel (1fr flexible)

## Form Structure

| Section | Key IDs | Type |
|---------|---------|------|
| Engagement Details | `partnerName`, `customerName`, `preparedBy`, `startDate`, `duration` | text, date, select |
| Customer Environment | `userCount`, `license`, `industry`, `maturity` | select × 4 |
| P1 Foundation Services | `#p1` container | checkbox group |
| P2 Intermediate Services | `#p2` container | checkbox group |
| P3 Advanced Services | `#p3` container | checkbox group |
| Engagement Type | `engType`, `ms_monitor`, `ms_tuning`, `ms_reporting`, `ms_incidents` | select + checkboxes |

## Key Functions

| Function | Purpose |
|----------|---------|
| `generateSOW()` | Main entry — reads form, builds 11-section SOW HTML, injects into `#sowDoc` |
| `getServices()` | Returns `[{label, effort, tier}]` from checked service checkboxes |
| `getManagedServices()` | Returns descriptive strings for checked `ms_*` checkboxes |
| `updateLicenseGating()` | Per-checkbox lock/unlock based on license dropdown and `data-license` attribute |
| `updateEffortTotal()` | Live effort hours calculation from checked items' `data-effort` |
| `parseEffort(str)` | Parses effort strings like "12–20 hrs" into `[lo, hi]` |
| `toggleAll(groupId)` | Select/deselect all checkboxes in a tier group |
| `fmtDate(v)` | Formats date input to "Month Day, Year" or placeholder |
| `rows(items)` / `li(items)` / `phase(...)` | HTML builder helpers for tables and lists |
| `copySOW()` | Clipboard copy via `document.execCommand('copy')` with fallback |
| `resetView()` | Hide SOW, re-show placeholder |

## Key Patterns

- **MIP + DLP services** in 3 priority tiers (P1/P2/P3), aligned with Microsoft's Data Security Deployment Guide slide 7
  - **P1 (8 items):** Foundation — included with M365 Business Premium (audit, labeling, label policies, basic DLP)
  - **P2 (1 item):** Intermediate — retention policies for Exchange (included with BP)
  - **P3 (15 items):** Advanced — auto-labeling, custom SITs, encryption, endpoint/Teams/Copilot DLP, DSPM, DKE, scanner, trainable classifiers, Content/Activity Explorer, container labels, MIP SDK, analytics
- Each checkbox carries `data-effort` (e.g. "12–20 hrs"), `data-tier` (1/2/3), and `data-license` ("bp"|"purview") attributes
- Per-checkbox license gating: items with `data-license="purview"` are locked (dimmed + 🔒 badge) when license is BP-only
- Managed services are separate checkboxes with `id="ms_*"` pattern
- SOW sections 1–11: Executive Summary, Objectives, Scope, Deliverables, Timeline, Partner Responsibilities, Customer Responsibilities, Assumptions, Out of Scope, Investment Summary, Acceptance
- Sections 4 (Deliverables) and 5 (Timeline) conditionally add rows based on selected tiers/managed services
- Investment Summary uses `[$ Add Amount]` placeholders — pricing is never auto-calculated
- Validation: at least 1 service must be selected or `generateSOW()` alerts and returns early
- License dropdown scoped to: Business Premium, BP + Purview Suite, BP + Defender & Purview Suite

## Conventions

- Microsoft Fluent-inspired design: `#0078d4` primary blue, Segoe UI font, Fluent color tokens
- Tier badge colors: `.tier-1` green, `.tier-2` yellow, `.tier-3` red
- Form fields use `id` attributes for direct `getElementById` access (no query selectors or data binding)
- All text content is hardcoded in JS template literals inside `generateSOW()`
- Empty form fields render as bracketed placeholders (e.g. `[Partner Company]`, `[Start Date TBD]`)

## When Modifying

- **Add a MIP service:** add `<label class="checkbox-item">` with `<input type="checkbox" value="..." data-effort="..." data-tier="1|2|3">` inside the appropriate `#p1`, `#p2`, or `#p3` group
- **Add a managed service:** add checkbox with `id="ms_<name>"` in the managed services section; update `getManagedServices()` to map the new ID to a descriptive string
- **Add a SOW section:** add to the HTML template in `generateSOW()` and update all subsequent section numbers
- **Change styling:** edit the `<style>` block — no external CSS files
- **Test:** open `mip-sow-generator.html` directly in a browser (no server needed)
- **Note:** P1 and P3 have "Select / Deselect All" buttons (P2 has only 1 item)
