# MIP SOW Builder

A single-page tool for Microsoft channel partners to generate Statement of Work (SOW) documents for **Microsoft Purview Information Protection (MIP)** engagements.

**[Launch the tool →](https://hiteshjo.github.io/MIP-SOW-Builder/mip-sow-generator.html)**

## Features

- **Three-tier service catalog** — Foundation (P1), Intermediate (P2), and Advanced (P3) MIP services with effort estimates
- **11-section SOW generation** — Executive Summary, Objectives, Scope, Deliverables, Timeline, Responsibilities, Assumptions, Out of Scope, Investment Summary, and Acceptance
- **Managed services add-on** — Optional ongoing monitoring, tuning, reporting, and incident response
- **Print / Copy** — Save as PDF or paste into your company template
- **Zero dependencies** — Pure HTML + CSS + vanilla JS in a single file; no build step, no backend

## Usage

Open `mip-sow-generator.html` in any modern browser, fill in the form, select services, and click **Generate SOW Document**.

### Running locally

```
# just open the file directly
start mip-sow-generator.html        # Windows
open mip-sow-generator.html         # macOS
xdg-open mip-sow-generator.html     # Linux
```

### Hosted on GitHub Pages

This repo is configured to deploy automatically via GitHub Pages. After enabling Pages in your repo settings (source: **GitHub Actions**), every push to `main` publishes the tool at:

```
https://<your-username>.github.io/MIP-SOW-Builder/mip-sow-generator.html
```

## Project Structure

```
mip-sow-generator.html   # The entire application (HTML + CSS + JS)
AGENTS.md                 # AI agent instructions for this codebase
README.md                 # This file
.github/workflows/        # GitHub Pages deployment workflow
```

## Contributing

1. Edit `mip-sow-generator.html` — all code lives in this single file
2. Open it in a browser to test your changes
3. Push to `main` — GitHub Pages deploys automatically