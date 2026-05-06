# AGENTS.md

## Project Overview

Single-page HTML tool for Microsoft channel partners to generate Statement of Work (SOW) documents for **Microsoft Purview Information Protection (MIP)** engagements. Everything lives in one self-contained file: [mip-sow-generator.html](mip-sow-generator.html).

## Architecture

- **No build system, no dependencies, no frameworks** — pure HTML + CSS + vanilla JS in a single file
- Left panel: multi-step form (engagement details → environment → service selection → engagement type)
- Right panel: generated SOW document with print/copy actions
- CSS is in a `<style>` block; JS is in a `<script>` block at the bottom

## Key Patterns

- **MIP services** are organized into 3 priority tiers (P1 Foundation, P2 Intermediate, P3 Advanced), each rendered as a checkbox group (`#p1`, `#p2`, `#p3`)
- Each service checkbox carries `data-effort` and `data-tier` attributes used during SOW generation
- Managed services are separate checkboxes with `id="ms_*"` pattern
- `generateSOW()` is the main entry point — reads form state, builds HTML string, injects into `#sowDoc`
- Print support via `@media print` rules that hide the form panel

## Conventions

- Microsoft Fluent-inspired design: `#0078d4` primary blue, Segoe UI font, Fluent color tokens
- Form fields use `id` attributes for direct `getElementById` access (no query selectors or data binding)
- SOW sections are numbered 1–11 and follow a fixed document structure
- All text content is hardcoded in the JS template literals inside `generateSOW()`

## When Modifying

- To add a new MIP service: add a `<label class="checkbox-item">` with `<input type="checkbox" value="..." data-effort="..." data-tier="1|2|3">` inside the appropriate `#p1`, `#p2`, or `#p3` group
- To add a new SOW section: add to the HTML template in `generateSOW()` and update section numbering
- To change styling: edit the `<style>` block — no external CSS files
- To test: open `mip-sow-generator.html` directly in a browser (no server needed)
