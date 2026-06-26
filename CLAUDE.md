# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Static marketing website for Ape Robotics. No build step, no package manager, no framework — just files served directly in a browser.

## Files

| File | Purpose |
|------|---------|
| `index.html` | Production landing page — self-contained HTML/CSS/JS, no runtime dependencies |
| `Ape Robotics Website.dc.html` | Design-tool version of the same page, authored in the `.dc.html` format |
| `support.js` | **Do not edit.** Generated dc-runtime bundle. Rebuild with `cd dc-runtime && bun run build` |
| `assets/bittle-x-sim.png` | Showcase screenshot |

## Two-file architecture

The site exists in two parallel forms:

**`index.html`** uses CSS classes and a `<style>` block. This is the file to edit for production content or styling changes.

**`Ape Robotics Website.dc.html`** uses the dc-runtime format (`<x-dc>` root element, `<helmet>` for head injection, all styles inline). It loads `support.js` which bootstraps React from unpkg and renders the component tree. This file is for design tooling (live preview/editing in a dc-compatible editor).

When updating copy or layout, apply changes to **both files** to keep them in sync.

## dc-runtime format (`.dc.html` files)

- The entire template lives inside `<x-dc>…</x-dc>`
- `<helmet>` injects children into `<head>` (stylesheets, scripts, meta)
- `{{expr}}` interpolates values; `<sc-if value="expr">`, `<sc-for list="expr" as="item">` for control flow
- `<dc-import name="ComponentName">` embeds a sibling `.dc.html` component
- `<x-import from="url.js" component="ExportName">` embeds an external React component
- Logic class (optional): `class Component extends DCLogic { renderVals() { return {...} } }`
- `support.js` exposes `window.__dcUpdate`, `window.__dcBoot`, `window.getDC`, `window.DCLogic`

## Development

Open `index.html` directly in a browser — no server required for the production page.

For the `.dc.html` file, serve the directory over HTTP (e.g. `python3 -m http.server`) so sibling component fetches work.
