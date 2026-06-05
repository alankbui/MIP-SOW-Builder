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

<!-- clarity-begin -->
<!-- clarity-meta
schema_version: 2
mode: userspace
protocol_dir_name: Clarity Protocol
processes_dir: C:/Program Files/Clarity/_internal/processes
-->
<!-- Clarity manages this block; edits between the clarity-begin / clarity-end markers will be overwritten on the next project open. Put project-specific guidance outside the markers. -->

## Clarity Protocol

This project uses the Clarity Protocol for structured thinking about consequential decisions — what to build and why, how it should be designed, where it might fail. Protocol documents live in `Clarity Protocol/`. A Clarity MCP server is configured for this project (see `.vscode/mcp.json`). Use its tools to interact with the protocol — do not try to install or run Clarity directly.

### When to engage

**Before building — think when it matters.** Two triggers:

1. *The user asks.* When they want to explore what to build, clarify requirements, brainstorm risks, or work through a decision: call the `run_clarity` MCP tool.

2. *You recognize an inflection point.* Before making choices that would be expensive to reverse — new services, auth/trust models, data schemas, external integrations, significant API contracts — call `check_decision` with what you plan to do. It returns existing decisions, requirements, and architecture so you can check for conflicts. Don't interrupt for routine implementation. The test: "If this turns out wrong, is it a 5-minute fix or a multi-day rework?" Interrupt for the latter.

**After building — keep the record current.** After significant implementation work (new features, architectural changes), call `get_packet_status` to find stale protocol documents. Update them with `read_protocol_document` / `write_protocol_document`. Record significant choices with `record_decision`; add risks with `record_failure`.

### Behaviors (apply throughout)

**Move quickly through what's obvious.** Many processes have multiple steps, but sometimes the answer to a step is already clear from context. When it is, just do it — write the result and present a summary for confirmation. Don't stop to ask permission at every small step. The goal is a natural conversation, not a checklist. Reserve interactive discussion for genuine ambiguity, tradeoffs, or decisions that need the user's judgment.

**Keep outputs narrative but brief.** All `.md` files in this project — protocol documents, process guides, and instructions — are repeatedly read by both humans and LLMs. They should read as smooth, concise narrative: easy to understand on first read, with nothing that wastes the reader's attention. A reader must immediately understand both the "what" and the "why." Cut anything that doesn't carry meaningful information. Since many of these files are instructions for LLMs which create further `.md` files, they should encourage the same discipline.

**Use `Clarity Protocol/notes.md` as shared memory.** At the start of every process, read `Clarity Protocol/notes.md` for guiding principles and cross-phase observations. When you notice something worth remembering — a design philosophy, a team constraint, an insight relevant to a future phase — add it. Tag actionable items for a specific phase with `[for: <phase>]` (e.g., `[for: failure-analysis] Authentication is a single point of failure`). When acting on a tagged item, remove it. Keep the file compact: consolidate redundant entries and remove items that have been absorbed into the relevant protocol documents.

**Generate threat model artifacts.** When writing or updating `solution/architecture.md`, include a Mermaid threat model diagram directly in the file as a fenced ` ```mermaid ` block — the packet generator extracts it automatically. Write the diagram yourself; you'll produce a better diagram than any code generator. Also write `Clarity Protocol/system-design.json` with structured component/flow/threat data for tooling. After failure brainstorming or analysis, write `Clarity Protocol/threat-model.md` — a concise threat model summary (1-2 pages max) with top risks, severities, one-line mitigations, and single points of failure.

<!-- clarity-end -->
