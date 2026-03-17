# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

JSK AI Automation & Digital Solutions — a B2B AI automation and digital transformation services company website targeting MSMEs. The main site is `index.html` (~4300 lines containing HTML, CSS, and JavaScript) with a Chinese translation at `index-zh.html`. The repo also contains a blog, supporting pages, and Python document generators.

## Tech Stack

- **Pure vanilla HTML/CSS/JS** — no frameworks, no bundlers, no package manager
- **Google Fonts** (Inter) loaded via CDN
- **Calendly** widget (CSS + async JS from `assets.calendly.com`) for booking calls
- **Python** (`python-docx`, `Pillow`) — for generating Word documents (resumes, cover letters, LinkedIn articles)
- **No build step** — HTML files are served directly as static content

## Development

Open `index.html` in a browser. There is no dev server, build command, or test suite. VS Code launch config (`.vscode/launch.json`) is set up for Chrome debugging.

**Deployment:** Netlify with custom domain `jskai.ai` (DNS via Netlify nameservers, domain registered on GoDaddy).

**Python generators:** Run with `python generate_<name>.py` — requires `python-docx` and `Pillow` (`pip install python-docx Pillow`).

## Site Structure

```
index.html              — Main English site (~4300 lines)
index-zh.html           — Chinese translation (mirrors English content)
signin.html             — Active customers landing page
privacy.html            — Privacy policy (GA4: G-YG9V32YW06)
robots.txt              — Search engine directives
blog/
  index.html            — Blog listing page (7 articles)
  automate-invoice-processing.html
  whatsapp-crm-vs-traditional-crm.html
  what-is-agentic-ai.html
  5-tasks-automate-2026.html
  ai-agents-small-business.html
  chatbots-vs-human-support.html
  whatsapp-crm-guide.html
proposals/              — Client proposals
linkedin_images/        — Generated banner images for LinkedIn articles
generate_*.py           — Python scripts for generating Word documents
```

## Architecture (index.html)

Organized in this order:

1. **`<head>` → `<style>` block** (~line 15–800) — All CSS including design tokens, component styles, responsive breakpoints
2. **`<body>` markup** (~line 800–3400) — Semantic sections connected by anchor links for smooth scrolling
3. **`<script>` block** (~line 3400–3470) — Vanilla JS for sticky nav, scroll-reveal animations, hamburger menu, FAQ accordion, portfolio filter
4. **AI Chatbot widget** (~line 3470–4050) — Self-contained block with its own `<style>` and `<script>`, streams responses from an external API
5. **WhatsApp floating button** (~line 4050–4150) — Self-contained block with its own `<style>`

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

Nav → Hero (with no-jargon messaging) → Logo Bar (tech logos) → What We Automate → Results You Can Expect (before/after metrics) → Services (tiered: AI Discovery/Growth/Scale) → Why JSK AI (5 cells including "Zero Jargon") → Use Cases (3 case studies) → Portfolio Showcase (filterable cards) → Industries (6 cards) → Comparison Table (JSK AI vs Developer vs Off-the-Shelf) → FAQ (accordion) → About Us → CTA (with satisfaction guarantee) → Footer → AI Chatbot (floating widget) → WhatsApp Button (floating)

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
- **Google Analytics** — GA4 tag `G-YG9V32YW06`

## Bilingual Site

`index-zh.html` mirrors all content from `index.html` in Chinese. When making changes to `index.html`, always update `index-zh.html` to match. Both files share the same CSS design tokens and JS features.

## Conventions

- All styling uses CSS custom properties for consistency; do not use hardcoded color values
- Responsive typography uses `clamp()` for fluid scaling
- Button variants follow the `.btn` / `.btn-dark` / `.btn-green` / `.btn-outline` pattern
- Section headers use `.section-tag`, `.section-title`, `.section-sub` classes
- Scroll-animated elements get the `.reveal` class
- Self-contained widgets (chatbot, WhatsApp) each have their own `<style>` and `<script>` blocks appended after the main content
- Blog articles follow a consistent template with nav, hero banner, article body, related posts, and footer
- Do not include the GitHub repository URL anywhere in the site or documents
- Python generators output to root for LinkedIn articles; job application docs are kept outside this project
