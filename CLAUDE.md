# Portfolio (Static Site)

Personal portfolio for Francis Padron. Single-file static HTML, deployed to GitHub Pages.

## What lives here
- `index.html` — the entire site (HTML + inline `<style>` + inline `<script>`). ~2000 lines.
- Image assets in the repo root (project screenshots, company logos, OG image, favicons).
- That's it. No build step, no `node_modules`, no framework.

## Live URL
https://padronfrancis1.github.io/portfolio/

## Sibling repo
The chat embedded near the top of the page is a separate Next.js app loaded via `<iframe>`. It lives at:
`C:\repositories\portfolio-agent`

## Which repo to edit (read this before changing anything)

| If you want to change… | Edit which repo |
|---|---|
| Hero, logos, projects cards, architecture SVG, experience, footer | **portfolio** (this repo) |
| OG / Twitter meta tags, LinkedIn share preview | **portfolio** (this repo) |
| Message bubbles, thinking indicator, suggested-questions sidebar | **portfolio-agent** (`frontend/`) |
| Agent loop, system prompt, tools, corpus, models | **portfolio-agent** (`backend/`) |

**Heuristic**: anything *outside* the chat iframe is here; anything *inside* the chat iframe is in `portfolio-agent`.

## How to deploy

Just push to `main`. GitHub Pages auto-publishes within ~1 min. No build, no Vercel.

```bash
git add index.html
git commit -m "feat(...): ..."
git push
```

If you change `og-companies.png` (the LinkedIn share preview), you also need to flush LinkedIn's cache by pasting the URL into <https://www.linkedin.com/post-inspector/> and clicking Inspect — they cache OG previews aggressively.

## Section anatomy (top to bottom)
1. **Hero** — bio + experience stats (`#hero`, `.hero-exp` list)
2. **Logos** — companies worked at (`#logos`)
3. **Chat** — iframe embedding portfolio-agent (`#ai-demo`, `#chat-section-iframe`)
4. **Architecture diagram** — SVG with separate desktop + mobile variants (`#ai-architecture`)
5. **Featured projects** — 4 large cards with screenshots + lightbox (`#systems`, `.proj-card`)
6. **Public repos** — compact tiles linking to GitHub (`#repos`, `.repo-card`)
7. **Working chronology** — experience timeline (`#experience`, `.exp-row`)
8. **Footer**

## Conventions

### All-SVG diagrams
Never mix HTML cards with SVG connector lines (coordinate spaces don't agree → lines won't connect). The architecture diagram is a single self-contained `<svg>` per breakpoint:
- `.arch-svg-desktop` (viewBox 760×240, horizontal fan-out) — shown on ≥768px
- `.arch-svg-mobile` (viewBox 360×600, vertical trunk + alternating tools) — shown on <768px

### Image lightbox
Project screenshots wrap in `<a class="proj-image-link">`. The IIFE near the bottom of the file hijacks clicks and opens `<dialog id="img-lightbox">`. Each anchor's `href` doubles as the new-tab fallback when `<dialog>.showModal` isn't available.

### Mobile breakpoints
- `768px` — tablet → phone (single-col grids, vertical arch SVG)
- `480px` — phone tightening (stats block stacks 01+label / number+years)

### Suggested questions
Buttons on the right of the chat (`.chat-prompt`) post `{type: 'portfolio:ask', text}` to the iframe via `postMessage`. The chat listens via `ExternalAskListener` in `portfolio-agent`.

### Project cards
Two flavors:
- **Featured** (`.proj-card` in `#systems`) — large, with screenshot + lightbox + highlighted stats (`<span class="hl-stat">`)
- **Public repos** (`.repo-card` in `#repos`) — compact 3-col tiles, just title + 1-line + tag + GitHub link

### Private code
Don't link to a private GitHub repo (404 = recruiter bounce). Use `<span class="proj-private-tag">Code: private — walkthrough on request</span>`.

### OG meta tags
For LinkedIn / X / Slack previews. Live in `<head>` under the "Open Graph / LinkedIn / X share preview" comment. Image is `og-companies.png`.

## Don't
- Don't add a build step. Keep it single-file static.
- Don't link to private GitHub repos. Use the private tag.
- Don't crop project screenshots — `object-fit: contain` is intentional (recruiters need to see chart numbers).
- Don't put text content in the HTML that contradicts `portfolio-agent/backend/corpus/resume.md` — the chatbot will surface the discrepancy. Update both together.
