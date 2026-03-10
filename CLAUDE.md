# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

JSK AI Automation & Digital Solutions — a single-page static website for a B2B AI automation and digital transformation services company. The entire site lives in one file: `index.html` (~3400 lines containing HTML, CSS, and JavaScript).

## Tech Stack

- **Pure vanilla HTML/CSS/JS** — no frameworks, no bundlers, no package manager
- **Google Fonts** (Inter) loaded via CDN
- **Calendly** widget (CSS + async JS from `assets.calendly.com`) for booking calls
- **No build step** — the file is served directly as static HTML

## Development

Open `index.html` in a browser. There is no dev server, build command, or test suite. VS Code launch config (`.vscode/launch.json`) is set up for Chrome debugging.

For deployment, the repo targets **Netlify** or **GitHub Pages** as static hosting.

**Repository:** https://github.com/lsgoplaan-droid/JSKportal.git

## Architecture

Everything is in `index.html`, organized in this order:

1. **`<head>` → `<style>` block** (~line 15–700) — All CSS including design tokens, component styles, responsive breakpoints
2. **`<body>` markup** (~line 700–2700) — Semantic sections connected by anchor links for smooth scrolling
3. **`<script>` block** (~line 2702–2767) — Vanilla JS for sticky nav, scroll-reveal animations, hamburger menu, FAQ accordion, portfolio filter
4. **AI Chatbot widget** (~line 2769–3334) — Self-contained block with its own `<style>` and `<script>`, streams responses from an external API
5. **WhatsApp floating button** (~line 3336–3431) — Self-contained block with its own `<style>`

### CSS Design Tokens (custom properties on `:root`)

| Token | Value | Purpose |
|-------|-------|---------|
| `--purple` | `#6366f1` | Primary brand color |
| `--green` | `#00e882` | Accent / success color |
| `--bg` | `#f7f7f0` | Page background (cream) |
| `--bg-dark` / `--bg-dark2` | `#0b0b14` / `#13132a` | Dark section backgrounds |
| `--text` / `--text-2` / `--text-3` | `#0a0a12` / `#48485e` / `#9090aa` | Text hierarchy |
| `--border` | `#e2e2ea` | Borders and dividers |
| `--purple-soft`, `--green-soft`, `--blue-soft`, etc. | Pastel variants | Card/badge backgrounds |
| `--radius-sm/md/lg/xl` | 8/12/16/24px | Border radius scale |
| `--shadow-card` / `--shadow-hover` | box-shadow values | Card elevation states |

### Responsive Breakpoints

`1400px` → `1000px` → `820px` (hamburger activates) → `600px` → `380px`

### Page Sections (in DOM order)

Nav → Hero → Logo Bar → Services (tiered: Starter/Growth/Scale) → How It Works (3 steps) → Why JSK AI (4 cells) → Use Cases (3 case studies) → Portfolio Showcase (filterable cards) → Industries (6 cards) → FAQ (accordion) → About Us → CTA → Footer → AI Chatbot (floating widget) → WhatsApp Button (floating)

### JavaScript Features

- **Sticky nav** — border/shadow added on scroll past 24px
- **Scroll reveal** — `IntersectionObserver` with staggered 90ms delay on `.reveal` elements
- **Hamburger menu** — toggle animation, body scroll lock, auto-close on link click
- **FAQ accordion** — one-open-at-a-time expand/collapse
- **Portfolio filter** — category-based show/hide via `.pf-filter-btn` and `data-category` attributes
- **AI Chatbot** — floating widget that streams responses via SSE from `jsk-ai-chatbot.onrender.com` (localhost:8000 in dev); maintains conversation history; has suggestion buttons, typing indicator, markdown rendering, and error fallback with contact links

### External Services

- **Calendly** — popup booking widget triggered via `Calendly.initPopupWidget()`
- **WhatsApp** — floating button linking to `wa.me/6591802686`
- **AI Chat API** — `https://jsk-ai-chatbot.onrender.com/api/chat` (production), `http://localhost:8000/api/chat` (development)

## Conventions

- All styling uses CSS custom properties for consistency; do not use hardcoded color values
- Responsive typography uses `clamp()` for fluid scaling
- Button variants follow the `.btn` / `.btn-dark` / `.btn-green` / `.btn-outline` pattern
- Section headers use `.section-tag`, `.section-title`, `.section-sub` classes
- Scroll-animated elements get the `.reveal` class
- Self-contained widgets (chatbot, WhatsApp) each have their own `<style>` and `<script>` blocks appended after the main content
