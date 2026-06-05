# MIP SOW Builder

A single-page tool for Microsoft channel partners to generate Statement of Work (SOW) documents for **Microsoft Purview Information Protection (MIP) & Data Loss Prevention (DLP)** engagements.

**[Launch the tool →](https://alankbui.github.io/MIP-SOW-Builder/data-security-sow-generator.html)**

## Features

- **Three-tier service catalog** — Foundation (P1), Intermediate (P2), and Advanced (P3) MIP services with effort estimates
- **11-section SOW generation** — Executive Summary, Objectives, Scope, Deliverables, Timeline, Responsibilities, Assumptions, Out of Scope, Investment Summary, and Acceptance
- **Managed services add-on** — Optional ongoing monitoring, tuning, reporting, and incident response
- **Print / Copy** — Save as PDF or paste into your company template
- **Zero dependencies** — Pure HTML + CSS + vanilla JS in a single file; no build step, no backend

## Usage

Open `data-security-sow-generator.html` in any modern browser, fill in the form, select services, and click **Generate SOW Document**.

### Running locally

```
# just open the file directly
start data-security-sow-generator.html        # Windows
open data-security-sow-generator.html         # macOS
xdg-open data-security-sow-generator.html     # Linux
```

### Hosted on GitHub Pages

This repo deploys automatically via GitHub Pages. Every push to `main` publishes the tool at:

```
https://alankbui.github.io/MIP-SOW-Builder/data-security-sow-generator.html
```

## Project Structure

```
data-security-sow-generator.html   # The entire application (HTML + CSS + JS)
AGENTS.md                           # AI agent instructions for this codebase
README.md                           # This file
.github/workflows/                  # GitHub Pages deployment workflow
```

## Contributing

1. Edit `data-security-sow-generator.html` — all code lives in this single file
2. Open it in a browser to test your changes
3. Push to `main` — GitHub Pages deploys automatically