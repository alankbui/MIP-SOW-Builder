# Changelog — MIP SOW Builder

All notable changes to `mip-sow-generator.html` are documented here.

---

## [Unreleased] — Active Development

---

## [2026-06-04] — Latest

### Changed — File Rename
- `mip-sow-generator.html` renamed to **`data-security-sow-generator.html`** to reflect the tool's expanded scope beyond MIP.

### Changed — Engagement Type Section Redesign
- Replaced the **Service Delivery Model** `<select>` dropdown with three independent checkboxes: One-Time Deployment, 30-Day Hypercare, and Ongoing Managed Services. Multiple options can be selected simultaneously; the SOW output joins selected values (e.g. `One-Time Deployment + 30-Day Hypercare`).
- **Monthly Health Monitoring** — renamed from "Monthly MIP Health Monitoring"; description expanded to cover DLP policy match rates, false positive trends, and user compliance alongside MIP label coverage and auto-labeling effectiveness.
- **Quarterly Policy Tuning** — description expanded to explicitly cover DLP policies, SITs, and auto-labeling rules alongside sensitivity label policies.
- **Incident Response Support** — description updated to cover DLP policy violations and false positive spikes in addition to MIP-related incidents.
- Managed service options (Monthly Health Monitoring, Quarterly Policy Tuning, Incident Response) are now **greyed out and disabled** until the Ongoing Managed Services checkbox is checked. A purple badge reads "Requires Ongoing Managed Services".
- Ongoing Managed Reports deliverable in SOW output updated from "MIP health and compliance summaries" to "data security health and compliance summaries".

### Removed
- **Compliance Reporting** managed service option removed from the Engagement Type section.

---

## [2026-06-04] — Earlier in session

### Changed — DLP Service Effort & Descriptions
- **DLP for Endpoints** (P3): effort updated from `12–20 hrs` to `16–28 hrs`. Description updated to include device onboarding validation, simulation mode testing and rule tuning, and alert/incident escalation configuration.
- **DLP for Teams** (P3): effort updated from `8–12 hrs` to `10–16 hrs`. Description updated to include simulation across internal and guest scenarios and alert/incident configuration.
- **DLP for Microsoft Copilot** (P3): effort updated from `8–12 hrs` to `10–16 hrs`. Description updated to include dual-rule requirement (SIT + label rules), simulation, and alert/incident configuration.

---

## [2026-06-02]

### Changed — DLP Policy: Exchange, SharePoint & OneDrive (P1)
- Folded DLP Policy Tuning & Simulation Mode and DLP Alerts & Incident Management into the base P1 DLP service. The service description now covers the full lifecycle: policy creation → simulation/tuning → enforcement → alert/incident management.
- Effort updated from `12–20 hrs` to `16–28 hrs` to reflect expanded scope.

### Removed
- **DLP Policy Tuning & Simulation Mode** (P2) — removed as a standalone item; incorporated into P1 DLP service.
- **DLP Alerts & Incident Management Configuration** (P2) — removed as a standalone item; incorporated into P1 DLP service.
- **DLP for Microsoft Fabric / Power BI** (P3) — removed. Rationale: M365 Business Premium does not include Power BI Pro; Microsoft Fabric targets enterprise capacity SKUs; low adoption in the SMB segment this tool serves. Preserved in `requirements.md` appendix for future consideration.

---

## [2026-06-01]

### Added — Analytics Consent Banner
- Added an **Analytics Consent** section at the top of the page (above the main form). Displays on every page load.
- Users can **Accept** or **Decline** Microsoft Clarity telemetry. Declining prevents Clarity from loading in the browser; accepting loads it dynamically.
- After a choice is made, the full banner collapses to a slim status strip showing the recorded preference and a note: "Refresh the page to update your choice."
- Preference is stored in `sessionStorage` — collapses for the remainder of the page session, resets on next page load or refresh.
- Clarity script removed from auto-loading in `<head>`; now loaded on demand only after explicit user acceptance.

### Added — Disclaimer Modal
- Added a **pop-up disclaimer modal** that appears on first page load each browser session (`sessionStorage`-gated).
- Modal blocks interaction with the tool until the user clicks "I Acknowledge & Continue".
- Disclaimer covers: intended use (Microsoft partners only, sample content), no Microsoft endorsement for legal or contractual use, user responsibility for content use, and notification that generated content may contain Organizational Identifiable Information (OII).

### Changed — MIP & DLP Maturity Dropdowns
- Split the single "Existing Data Security Maturity" dropdown into two separate dropdowns: **MIP Maturity** and **DLP Maturity**.
- MIP Maturity: renamed "Partial MIP deployment in place" to "Mixed - partial deployment"; removed "Labels deployed, no auto-labeling" option.
- DLP Maturity (new): four options — No existing policies (Greenfield), Policies created not yet deployed, Mixed - partial deployment, Mature deployment optimization needed.
- SOW Executive Summary now displays both maturity values.
- Greenfield assumption in SOW now only suppresses when **both** dropdowns are set to Greenfield.
- Save / load / reset all updated to include `dlpMaturity`.

---

## [2026-05-XX] — Initial Development Sprint

### Added — New DLP Services
- **DLP for Microsoft Edge / Inline Web Traffic** (P3, Purview Suite): prevents sensitive data from being sent to unmanaged AI applications (e.g., ChatGPT, Gemini) and personal cloud storage via Edge for Business browser enforcement. Effort: `8–12 hrs`.

### Changed — Section Renaming (R5)
- All form section headings renamed from "MIP Services: Foundation / Intermediate / Advanced" to **"Data Security Services: Foundation / Intermediate / Advanced"**.
- Page `<title>` and header updated accordingly.
- Maturity label renamed from "Existing Data Security Maturity" to reflect the domain split (see above).

### Fixed — Service License Gating Corrections (R1)
- **Container Label Configuration** — moved from P3 (Advanced) to P1 (Foundation). Correctly available with Business Premium.
- **Retention Policies for Exchange** — `data-license` corrected to `purview`; greyed out when license is BP-only.
- **Custom Sensitive Information Types (SITs)** — `data-license` corrected to `purview`.
- **MIP SDK Integration** — `data-license` corrected to `purview`.

### Changed — SOW Output (R3)
- Executive Summary, Objectives, and Scope of Services sections updated to reference both **Microsoft Purview Information Protection (MIP) and Data Loss Prevention (DLP)** rather than MIP only.

---

## Notes

- All changes are contained in the single file `data-security-sow-generator.html` — no build system or external dependencies.
- IRM (Insider Risk Management) services were scoped and researched but deferred to a future release. Full service catalog and effort estimates are preserved in `Clarity Protocol/goal/requirements.md` (Appendix).
- DLP for Microsoft Fabric / Power BI was evaluated and deferred; preserved in requirements appendix.
- Pricing is never auto-calculated in any version — `[$ Add Amount]` placeholders are intentional.
