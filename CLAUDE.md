# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

JSK AI Automation & Digital Solutions — a single-page static website for a B2B AI automation and digital transformation services company. The entire site lives in one file: `index.html` (~2500 lines containing HTML, CSS, and JavaScript).

## Tech Stack

- **Pure vanilla HTML/CSS/JS** — no frameworks, no bundlers, no package manager
- **Google Fonts** (Inter) loaded via CDN
- **No build step** — the file is served directly as static HTML

## Development

Open `index.html` in a browser. There is no dev server, build command, or test suite. VS Code launch config (`.vscode/launch.json`) is set up for Chrome debugging.

For deployment, the repo targets **Netlify** or **GitHub Pages** as static hosting.

**Repository:** https://github.com/lsgoplaan-droid/JSKportal.git

## Architecture

Everything is in `index.html`, organized in this order:

1. **`<style>` block** — All CSS including design tokens, component styles, responsive breakpoints
2. **`<body>` markup** — Semantic sections connected by anchor links for smooth scrolling
3. **`<script>` block** — Vanilla JS for sticky nav, scroll-reveal animations, hamburger menu, FAQ accordion

### CSS Design Tokens (custom properties on `:root`)

| Token | Value | Purpose |
|-------|-------|---------|
| `--purple` | `#6366f1` | Primary brand color |
| `--green` | `#00e882` | Accent color |
| `--bg` | `#f7f7f0` | Page background (cream) |
| `--bg-dark` / `--bg-dark2` | `#0b0b14` / `#13132a` | Dark section backgrounds |
| `--text` / `--text-2` / `--text-3` | `#0a0a12` / `#48485e` / `#9090aa` | Text hierarchy |
| `--radius-sm/md/lg/xl` | 8/12/16/24px | Border radius scale |

### Responsive Breakpoints

`1400px` → `1000px` → `820px` (hamburger activates) → `600px` → `380px`

### Page Sections (in DOM order)

Nav → Hero → Logo Bar → Services (6 cards) → How It Works (3 steps) → Why JSK AI (4 cells) → Use Cases (3 case studies) → Industries (6 cards) → FAQ (accordion) → About Us → Integrations → CTA → Footer

### JavaScript Features

- **Sticky nav** — border/shadow added on scroll past 24px
- **Scroll reveal** — `IntersectionObserver` with staggered 90ms delay on `.reveal` elements
- **Hamburger menu** — toggle animation, body scroll lock, auto-close on link click
- **FAQ accordion** — one-open-at-a-time expand/collapse

## Conventions

- All styling uses CSS custom properties for consistency; do not use hardcoded color values
- Responsive typography uses `clamp()` for fluid scaling
- Button variants follow the `.btn` / `.btn-dark` / `.btn-green` / `.btn-outline` pattern
- Section headers use `.section-tag`, `.section-title`, `.section-sub` classes
- Scroll-animated elements get the `.reveal` class
